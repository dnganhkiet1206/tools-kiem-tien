# YAAS 00 — Kiến trúc tổng thể & State Machine

## Goal
Định nghĩa kiến trúc chuẩn của Content Factory: các tầng, luồng dữ liệu, state machine sản xuất một video từ ý tưởng đến analytics — để mọi chapter sau chỉ là hiện thực hoá từng khối, và mọi AI đọc spec đều dựng ra cùng một hệ.

## Background
Sản xuất video là pipeline nhiều bước phụ thuộc tuần tự (script → voice → visual → edit → publish), mỗi bước có thể hỏng độc lập, chi phí mỗi bước khác nhau (LLM local ~0, video gen đắt), và chất lượng đầu ra bị chính sách nền tảng ràng buộc (xem `01`). Bài toán vì thế không phải "gọi AI nào" mà là **điều phối có trạng thái, có retry, có cổng kiểm định** — đúng dạng bài state machine + queue, không phải dạng bài "một prompt to".

## Theory — 4 nguyên tắc thiết kế
1. **Local-first, cloud-fallback:** mọi bước chạy được bằng open source local trước; cloud API chỉ khi (a) chất lượng local dưới ngưỡng chấp nhận của bước đó, và (b) ROI dương đo được (chi phí API/video < giá trị tăng thêm). Quy tắc nâng cấp cụ thể theo từng pipeline chapter 03–06.
2. **Human gate ở 2 điểm cố định:** sau Fact Check (duyệt script) và sau Editing (duyệt video trước publish). Đây là ràng buộc chính sách + chất lượng (xem `01`), không phải tuỳ chọn. Mọi bước khác được phép chạy không giám sát.
3. **Mọi artifact là file trên đĩa + metadata trong DB:** script.md, voice.wav, cut.mp4… đặt tên theo `{video_id}/{state}/`; trạng thái, thời gian, chi phí, lỗi ghi vào SQLite. Chạy lại bất kỳ state nào từ artifact của state trước — không bao giờ làm lại từ đầu vì hỏng một bước.
4. **Idempotent + resumable:** mỗi state chạy lại không phá dữ liệu cũ (ghi version mới); crash giữa chừng → Recovery đọc DB và tiếp tục từ state dở.

## Architecture

```
┌─────────────────────────── ORCHESTRATION (n8n + Python + cron) ───────────────────────────┐
│  Queue (SQLite table)  ·  State Machine  ·  Retry/Backoff  ·  Webhook (human approve)      │
└──────┬──────────────┬───────────────┬───────────────┬──────────────┬──────────────────────┘
       ▼              ▼               ▼               ▼              ▼
  RESEARCH        SCRIPT          VOICE          VISUAL         PUBLISH
  SearXNG/API    LLM local       TTS local      Image gen      YouTube Data API
  + scraping     (Ollama)        (Kokoro/…)     + stock        + scheduler
  + vector DB    cloud fallback  cloud fallback + FFmpeg
       │              │               │               │              │
       └──────────────┴───────┬───────┴───────────────┴──────────────┘
                              ▼
             STORAGE: filesystem artifacts + SQLite (state,cost,log) + vector DB (research/memory)
                              ▼
             ANALYTICS LOOP: YouTube Analytics API → DB → báo cáo tuần → cập nhật Planning
       ▲                                                                        │
       └────────────────────── HUMAN GATES: approve script · approve video ─────┘
```

Thành phần tối thiểu (đủ chạy từ ngày 1, tất cả open source): Python 3.11+, FFmpeg, Ollama, n8n (self-host) hoặc thuần Python+cron, SQLite, Git. Docker khuyến nghị nhưng không bắt buộc trên laptop yếu. Vector DB (Qdrant/Chroma) chỉ thêm khi kho research vượt vài trăm tài liệu — trước đó SQLite FTS đủ `[T1: SQLite FTS5 docs]`.

## Workflow (một video, tổng quát — chi tiết từng bước ở chapter 03–08)
Idea backlog → Planning (chọn đề tài theo dữ liệu) → Research (nguồn + fact) → Script (LLM + template) → Fact Check → **HUMAN GATE 1** → Voice (TTS) → Image/Video assets → Editing (assembly) → Thumbnail → SEO metadata → **HUMAN GATE 2** → Publishing (API, đúng lịch) → Analytics (48h, 7d, 28d) → Optimization (cập nhật template/đề tài) → vòng lặp.

## Input
Idea backlog (≥20 đề tài đã chấm điểm), cấu hình kênh (ngách, persona giọng, template visual), phần cứng theo `02`.

## Output
Video công bố đúng lịch + toàn bộ artifact trung gian có version + bản ghi chi phí/thời gian mỗi state + số liệu analytics chảy ngược về Planning.

## Dependencies
Phần cứng (`02`) · chính sách & chiến lược kênh (`01`) · YouTube Data API v3 quota mặc định 10.000 units/ngày, upload ~1.600 units/video `[T1: Google API docs — VERIFY quota hiện hành]` · điện + internet ổn định cho batch đêm.

## Decision Tree — chọn mức kiến trúc khởi điểm

```
IF mới bắt đầu, chưa có video nào
THEN KHÔNG dựng n8n/Docker/queue vội — chạy pipeline bằng 1 script Python tuần tự + checklist tay
     (dựng hạ tầng trước khi biết format thắng = automation hoá thứ chưa đáng tự động,
      cùng logic `ai-money-os/03` §10)
ELSE-IF đã có 5–10 video, format ổn định, mỗi video >3h công tay
THEN dựng state machine + n8n theo `07`, tự động hoá bước tốn giờ nhất trước
ELSE-IF >2 kênh hoặc >3 video/tuần
THEN thêm queue đa kênh + Docker hoá + tách máy/GPU thuê theo `09`
```

## State Machine — 20 trạng thái

Quy ước chung: mọi state ghi `(video_id, state, started_at, ended_at, cost, artifact_path, error)` vào DB; transition chỉ xảy ra khi Validation của state đạt; fail → `Retry` (tối đa N lần, backoff) → `Error` → `Recovery`.

| State | Entry Condition | Input | Output | Exit / Transition | Validation |
|---|---|---|---|---|---|
| Planning | backlog có đề tài chưa làm | backlog + analytics kỳ trước | 1 đề tài + góc tiếp cận + format | → Research | đề tài đạt điểm ngưỡng theo `01` §Decision Tree |
| Research | đề tài đã chốt | đề tài | kho nguồn có trích dẫn (≥5 nguồn, ≥2 Tier 1–2) | → Validation | mỗi claim chính có ≥1 nguồn; nguồn có URL + ngày |
| Validation | research xong | kho nguồn | verdict: đủ/không đủ làm video | đủ → Script · thiếu → Research (1 lần) → loại đề tài | không mâu thuẫn giữa các nguồn chính; đề tài chưa bão hoà (check top 10 kết quả YouTube) |
| Script | validation đạt | kho nguồn + template kịch bản | script.md (hook, thân, CTA, timestamps) | → Fact Check | đúng cấu trúc template; độ dài khớp format; không đoạn nào thiếu nguồn |
| Fact Check | script xong | script + kho nguồn | script đã đánh dấu từng claim ✓/✗/? | ✗ hoặc ? → Script (sửa) · sạch → Review | 100% claim định lượng có nguồn đối chiếu; LLM checker ≠ LLM writer (chapter `03`) |
| Review | fact check sạch | script final | approve / edit / reject | **HUMAN GATE 1** → Voice · reject → Planning | người đọc toàn bộ; sửa giọng điệu, cắt lê thê; ký duyệt trong DB |
| Voice | script approved | script (đã chèn SSML/pause) | voice.wav + timestamps từng câu | → Image | nghe kiểm 3 đoạn ngẫu nhiên; WER spot-check bằng STT (`04`); không hallucination audio |
| Image | voice xong | script + shotlist | bộ ảnh/b-roll theo timestamp | → Video | đủ asset cho 100% timeline; không ảnh vi phạm bản quyền (chỉ nguồn được phép — `05`) |
| Video | asset đủ | voice + assets + template | các segment video thô | → Editing | mọi segment render không lỗi; đồng bộ timestamp |
| Editing | segments xong | segments + nhạc + SFX | cut.mp4 hoàn chỉnh | → Thumbnail | duration đúng ±10%; audio level chuẩn (-14 LUFS `[T1: YouTube khuyến nghị loudness — VERIFY]`); không frame đen/câm |
| Thumbnail | cut xong | title + key visual | 2–3 ứng viên thumbnail | → SEO | đọc được ở kích thước nhỏ; đúng guideline kênh; người chọn 1 |
| SEO | thumbnail xong | script + keyword research | title, description, tags, chapters, disclosure AI | → Publishing (chờ GATE 2) | title ≤60 ký tự hiển thị; disclosure "altered/synthetic" bật nếu áp dụng `[T1: YouTube policy]` |
| Publishing | **HUMAN GATE 2** approve video+metadata | cut.mp4 + metadata + lịch | video scheduled/published trên YouTube | → Analytics | API trả về video_id; trạng thái đúng (scheduled/public); quota còn đủ |
| Analytics | video đã public ≥48h | video_id | số liệu 48h/7d/28d vào DB | → Optimization | đủ các KPI bắt buộc (CTR, AVD, retention curve) |
| Optimization | có số liệu ≥3 video cùng format | analytics DB | 1–3 thay đổi cụ thể cho template/backlog | → Planning (vòng mới) · đủ điều kiện → Scaling | mỗi thay đổi gắn với 1 số liệu, có giả thuyết viết trước |
| Scaling | trigger ở `09` đạt | toàn hệ đang chạy ổn | kênh/format/ngôn ngữ mới | → Planning (kênh mới) | không vượt trần rủi ro `ai-money-os/11` §5c |
| Retry | state bất kỳ fail lần ≤N | state + error log | chạy lại state | thành công → state kế · fail lần N → Error | backoff 2ˣ; N mặc định 3; lỗi giống hệt 2 lần → đổi tham số trước khi retry lần 3 |
| Error | retry cạn | error log | ticket trong DB + thông báo operator | → Recovery | lỗi được phân loại (tool/input/quota/policy) |
| Recovery | có ticket Error hoặc crash giữa chừng | DB state + artifacts | pipeline tiếp tục từ artifact tốt gần nhất | → state dở dang · không cứu được → Planning (loại video, ghi khám nghiệm) | không mất artifact đã có; nguyên nhân gốc ghi vào changelog |
| Completed | analytics 28d xong | toàn bộ record | video đóng hồ sơ; học liệu vào Optimization | (kết thúc vòng đời) | hồ sơ đủ: chi phí thật, giờ người thật, KPI |

## Failure Modes (mức kiến trúc — mức module xem từng chapter)
1. **Nghẽn ở human gate** (operator bận → hàng đợi ứ) → gate có SLA nội bộ (24h) + batch duyệt 1 lần/ngày; gate ứ >5 item là tín hiệu giảm nhịp sản xuất, không phải bỏ gate.
2. **Một tool chết/đổi license giữa pipeline** → mọi tool có ít nhất 1 alternative ghi sẵn trong chapter tương ứng; adapter pattern: mỗi bước gọi qua interface riêng (`tts.generate()`, không gọi thẳng model).
3. **Quota YouTube API cạn** → publishing tách khỏi sản xuất, có hàng đợi riêng, lịch upload phân bổ; không bao giờ để sản xuất chờ publish.
4. **Chi phí âm thầm phình** (cloud fallback bật mãi không tắt) → cột `cost` bắt buộc mỗi state; báo cáo tuần có tổng chi phí/video; trần cứng $/video theo `01` §Cost.
5. **Drift chất lượng** (template mòn, model đổi hành vi sau update) → pin version model/tool; mỗi update chạy 1 video benchmark so với video chuẩn trước khi dùng cho production.

## Recovery Strategy
Nguyên tắc duy nhất: **artifact + DB là nguồn sự thật, không phải trí nhớ của operator.** Mọi recovery = đọc DB tìm state cuối thành công → chạy lại từ đó. Backup: artifacts rsync sang ổ ngoài/remote hằng tuần; DB commit vào Git hằng ngày (SQLite file nhỏ).

## Automation
Tỷ lệ mục tiêu theo giai đoạn (đo bằng % bước không cần người trong 20 state): thủ công có checklist (video 1–5) → ~40% (script pipeline tự động, video 6–15) → ~70% (thêm voice+assembly, video 16+) → ~85% trần thiết kế (2 human gate + planning không bao giờ tự động — theo nguyên tắc Theory #2).

## Prompt Templates
Ở chapter của từng module (03–06). Chapter này chỉ định chuẩn: mọi prompt trong YAAS theo đúng 13 mục của spec gốc (Role → Revision Guide) và lưu trong repo dưới `prompts/{module}/{tên}.md` có version.

## Open Source Alternatives / Paid Alternatives / Benchmark / Cost / ROI
Theo từng module ở chapter 02–06. Mức kiến trúc: toàn bộ tầng orchestration chạy bằng open source thuần (n8n self-host license Sustainable Use — đọc kỹ nếu bán lại dịch vụ hosting `[T1: n8n license docs]`; thay thế thuần OSS: Python + APScheduler/cron). Chi phí hạ tầng khởi điểm: $0 (local) + điện.

## Checklist (dựng xong kiến trúc mới được làm video 1)
- [ ] Cây thư mục artifact + SQLite schema tạo xong (bảng: videos, states, costs, errors)
- [ ] Python env + FFmpeg + Ollama chạy được end-to-end "hello world" mỗi tầng
- [ ] 2 human gate định nghĩa rõ ai duyệt, ở đâu, SLA bao lâu
- [ ] Adapter interface cho LLM/TTS/Image (đổi tool không sửa pipeline)
- [ ] Trần chi phí/video và trần retry ghi vào config, không hardcode

## Validation (của chính chapter này)
Kiến trúc đạt nếu: một người mới đọc spec dựng lại được hệ trong 1–2 ngày; một video hỏng ở bước bất kỳ khôi phục được không mất artifact; đổi 1 tool bất kỳ không sửa quá 1 file adapter.

## Optimization
Mỗi 10 video: đo giờ-người/video theo state → tự động hoá đúng state tốn giờ nhất còn lại (không tự động hoá theo độ "ngầu" của công nghệ).

## Scaling
Xem `09`. Kiến trúc này scale bằng cách nhân queue + worker, không đổi state machine.

## Self Audit
1. *Khoảng trống?* Chưa đặc tả schema SQLite chi tiết và adapter interface code — sẽ nằm ở `07`. Quota API và chuẩn loudness cần VERIFY số hiện hành.
2. *OSS tốt hơn?* Orchestration bằng Temporal/Prefect mạnh hơn n8n về state machine thuần code, nhưng nặng hơn nhu cầu solo — giữ n8n/Python, ghi nhận alternative.
3. *Benchmark mới hơn?* Mức kiến trúc không có benchmark; module thì có (02+).
4. *Workflow đơn giản hơn?* Có: giai đoạn video 1–5 chính là bản đơn giản hoá (script tuần tự + checklist) — đã đưa vào Decision Tree làm mặc định.
5. *Tự động hoá thêm?* Human gate 1 có thể thu hẹp thành duyệt-diff (chỉ đọc phần thay đổi so với template) khi kênh đã >30 video — ghi vào `08`.

## Next Task
Đọc `01` để chốt ngách + format + ràng buộc chính sách TRƯỚC khi mua/cài bất kỳ thứ gì ở `02`.
