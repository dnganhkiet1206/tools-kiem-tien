# YAAS 02 — Phần cứng & Local AI Stack

## Goal
Chọn đúng stack model/tool local cho từng tầng (LLM, TTS, STT, Image, Video, OCR/Subtitle, Search) theo phần cứng đang có — với decision tree để người 8GB RAM và người có GPU 24GB đều dựng được factory, và luật nâng cấp trả phí có ROI.

## Background
Tính đến giữa 2026, khoảng cách chất lượng open-source vs thương mại đã thu hẹp mạnh ở TTS (từ ~223 xuống ~81 điểm ELO trên Artificial Analysis Speech Arena; một số model mở thắng ElevenLabs trong blind test) `[T2:` [TextToLab](https://texttolab.com/blog/open-source-text-to-speech), [Modal](https://modal.com/blog/open-source-tts)`]`, ở LLM cỡ nhỏ (7–20B đủ cho script ngách) `[T2:` [Microcenter guide](https://www.microcenter.com/site/mc-news/article/best-local-llms-8gb-16gb-32gb-memory-guide.aspx)`]`; riêng **video generation local vẫn là tầng đắt nhất** (model tốt cần 24–32GB VRAM) `[T2:` [Local AI Master](https://localaimaster.com/blog/local-ai-video-generation), [AI Magicx](https://www.aimagicx.com/blog/open-source-ai-video-models-comparison-2026)`]`. Chiến lược stack vì thế: **LLM + TTS local từ ngày 1; generative video là tuỳ chọn cuối cùng, không phải điều kiện bắt đầu.**

## Theory — 3 luật chọn tool
1. **Format quyết định stack** (`01` trước, chapter này sau): kênh voice-over + minh hoạ KHÔNG cần video gen — ảnh + stock + FFmpeg đủ; đừng mua GPU cho tính năng chưa cần.
2. **License là thông số kỹ thuật, không phải chú thích:** kênh kiếm tiền = commercial use. XTTS-v2 chất lượng tốt nhưng **non-commercial (CPML)** → loại khỏi production `[T1: Coqui license]` `[T2:` [TextToLab](https://texttolab.com/blog/open-source-text-to-speech)`]`. Chỉ nhận Apache-2.0 / MIT / tương đương cho artifact bán được.
3. **Quantization là đòn bẩy chính của laptop:** Q4_K_M giảm ~nửa bộ nhớ với mất mát chất lượng nhỏ; Q5_K_M là điểm cân bằng cho máy 16GB `[T2:` [PromptQuorum](https://www.promptquorum.com/local-llms), [Microcenter](https://www.microcenter.com/site/mc-news/article/best-local-llms-8gb-16gb-32gb-memory-guide.aspx)`]`.

## Architecture
Mọi model gọi qua adapter (`00` Theory): `llm.generate()` → Ollama (local) hoặc API (fallback); `tts.speak()` → Kokoro/Chatterbox hoặc API; đổi model = đổi config, không đổi pipeline.

## Decision Tree — theo phần cứng

```
TẦNG LLM (script, research, SEO — chapter 03/06)
IF RAM 8GB (mọi OS)
THEN Qwen 7B-class Q4 (~5GB) qua Ollama; chậm trên CPU (~8–12 tok/s) nhưng đủ cho batch đêm
     [T2: Microcenter]; nếu quá chậm → fallback API giá rẻ cho riêng bước script dài
ELSE-IF RAM 16GB
THEN Gemma 3 12B hoặc gpt-oss:20b (MoE, chạy tốt không GPU) Q4/Q5 [T2: PromptQuorum, Morph];
     đủ chất lượng cho 90% việc của factory
ELSE-IF RAM/VRAM ≥24–32GB (hoặc Mac unified memory)
THEN Qwen3-30B-class MoE Q4 (~19GB, context dài) [T2: Morph] — gần như không cần cloud cho text
FALLBACK API khi: fact-check đề tài nhạy cảm, script dài chất lượng thấp lặp lại 2 lần retry
     → dùng API theo trần $/video trong Charter

TẦNG TTS (chapter 04)
IF CPU-only / máy yếu
THEN Kokoro-82M (Apache 2.0, chạy CPU, ~2–3GB, thậm chí Raspberry Pi) — mặc định của YAAS
     [T2: Local AI Master, Modal]
ELSE-IF cần giọng tự nhiên hơn / voice cloning, có ~6GB+ VRAM
THEN Chatterbox (MIT, 0.5B; blind test 65.3% chọn nó thắng ElevenLabs — benchmark do chính
     Resemble công bố, coi là T2-có-lợi-ích) [T2: TextToLab, Modal]
ELSE-IF đa ngôn ngữ (tiếng Việt/10 ngôn ngữ) hoặc clone từ 3s mẫu
THEN Qwen3-TTS (Alibaba, 01/2026) [T2: BentoML, SiliconFlow] — VERIFY license bản phát hành
CẤM production: XTTS-v2 (non-commercial) · CHÚ Ý: kiểm tra hallucination audio — lỗi đặc trưng
     của TTS thế hệ này; QC bằng STT đối chiếu (dưới)

TẦNG STT / SUBTITLE (QC voice + phụ đề)
→ faster-whisper (CTranslate2) chạy CPU tốt, MIT [T1: repo docs]; whisper.cpp nếu cần nhẹ nữa

TẦNG IMAGE (minh hoạ, thumbnail nền — chapter 05/06)
IF không GPU
THEN SDXL/FLUX qua dịch vụ free-tier có hạn mức, HOẶC ảnh stock license rõ (Pexels/Wikimedia/
     Openverse) + đồ hoạ template (HTML/CSS render, matplotlib cho chart) — đủ cho docu/education
ELSE-IF VRAM 8–12GB  THEN SDXL / FLUX.1-schnell (Apache 2.0) local qua ComfyUI
ELSE-IF VRAM ≥16GB   THEN FLUX.1-dev (license non-commercial cho weights — đọc kỹ điều khoản
     khi dùng thương mại [T1: BFL license — VERIFY]) hoặc model Apache mới hơn

TẦNG VIDEO GENERATION (tuỳ chọn — KHÔNG phải điều kiện bắt đầu)
IF VRAM <8GB   THEN bỏ qua — dùng ảnh + Ken Burns + stock qua FFmpeg (chapter 05); chất lượng
     kênh docu không phụ thuộc video gen
ELSE-IF 8–16GB THEN Wan 1.3B (Apache 2.0, chạy từ 8GB) cho b-roll ngắn [T2: WhiteFiber, Hyperstack]
ELSE-IF 24GB   THEN Wan 14B quantized (chất lượng người/photoreal tốt nhất) hoặc LTX 13B
     (nhanh 2–3×, hợp iterate) [T2: AI Magicx, LTX blog]
ELSE-IF 32GB+  THEN LTX-2.x (video+audio đồng bộ, 4K — spec 32GB VRAM) [T2: Local AI Master]
CLOUD FALLBACK: thuê GPU theo giờ (RunPod/Vast-class) rẻ hơn mua GPU cho <10 clip/tuần — tính
     điểm hoà vốn trước khi mua phần cứng [nguyên tắc, giá INSUFFICIENT EVIDENCE — biến động]

TẦNG SEARCH/RESEARCH (chapter 03)
→ SearXNG self-host (meta-search, AGPL) hoặc API search free-tier; trafilatura/readability
  trích nội dung; lưu vào SQLite FTS → nâng Qdrant/Chroma khi >500 tài liệu
```

## Workflow (dựng stack — 1 buổi đến 1 ngày)
Cài Python 3.11+ + FFmpeg + Git → Ollama + pull model theo tier RAM → test LLM adapter → cài TTS theo tier → test 1 đoạn 200 từ + STT đối chiếu → (nếu có GPU) ComfyUI + image model → chạy smoke test end-to-end: 1 script 300 từ → voice → 3 ảnh → 1 video 60s bằng FFmpeg → ghi thời gian + đỉnh RAM/VRAM vào DB làm baseline.

## Input
Phần cứng thực tế (RAM, VRAM, OS, dung lượng đĩa ≥100GB trống khuyến nghị); format từ `01`.

## Output
File `config/stack.yaml`: model từng tầng + version pin + tham số + fallback; baseline benchmark của chính máy mình (quan trọng hơn benchmark trên mạng).

## Dependencies
Ollama, FFmpeg, Python; driver GPU đúng (CUDA/ROCm/Metal); dung lượng đĩa cho model (LLM 4–20GB, image model 6–24GB, video model 20GB+).

## Bảng đánh giá tool (schema rút gọn — đủ tiêu chí bắt buộc, chi tiết vận hành ở chapter module)

| Tool | Purpose | License | Mac/Win/Linux | Offline | API | Docker | Cost | Stage khuyến nghị |
|---|---|---|---|---|---|---|---|---|
| Ollama | Chạy LLM local | MIT | ✅/✅/✅ | ✅ | ✅ REST | ✅ | 0 | Ngày 1 |
| llama.cpp | LLM tối giản, kiểm soát sâu | MIT | ✅/✅/✅ | ✅ | ✅ server | ✅ | 0 | Khi cần tinh chỉnh hơn Ollama |
| Kokoro-82M | TTS nhẹ, CPU | Apache 2.0 | ✅/✅/✅ | ✅ | qua wrapper | ✅ | 0 | Ngày 1 |
| Chatterbox | TTS chất lượng + cloning | MIT | ✅/✅/✅ (GPU tốt hơn) | ✅ | ✅ | ✅ | 0 | Khi voice là điểm yếu số 1 |
| faster-whisper | STT/subtitle/QC | MIT | ✅/✅/✅ | ✅ | lib | ✅ | 0 | Ngày 1 |
| FFmpeg | Assembly video, audio | LGPL/GPL | ✅/✅/✅ | ✅ | CLI | ✅ | 0 | Ngày 1 |
| ComfyUI | Image/video gen workflow | GPL-3.0 | ✅/✅/✅ | ✅ | ✅ API | ✅ | 0 | Khi có GPU + cần visual gen |
| SDXL / FLUX.1-schnell | Image gen | OpenRAIL / Apache 2.0 | (theo ComfyUI) | ✅ | qua ComfyUI | ✅ | 0 | Video 6+ |
| Wan (1.3B/14B) | Video gen | Apache 2.0 | Linux tốt nhất | ✅ | qua ComfyUI | ✅ | 0 | Tuỳ chọn, VRAM ≥8/24GB |
| LTX | Video gen nhanh/4K | mở theo bản — VERIFY | Linux/Win | ✅ | ✅ | ✅ | 0 | Tuỳ chọn, VRAM lớn |
| n8n | Orchestration | Sustainable Use (đọc kỹ nếu bán hosting) | ✅/✅/✅ | ✅ self-host | ✅ | ✅ | 0 | Video 6–15 (`00` DT) |
| SearXNG | Meta-search research | AGPL-3.0 | ✅ (Docker) | ⚠️ cần internet | ✅ | ✅ | 0 | Khi research >10 truy vấn/ngày |
| Qdrant / Chroma | Vector DB | Apache 2.0 | ✅/✅/✅ | ✅ | ✅ | ✅ | 0 | Kho research >500 tài liệu |
| SQLite | State + FTS | Public domain | ✅/✅/✅ | ✅ | lib | — | 0 | Ngày 1 |

(Các cột Quality/Speed/Stability/Community/Maintenance: tất cả tool trên đều đạt mức production-dùng-rộng-rãi tại 07/2026 `[T2]`; đánh giá số hoá từng tool sẽ lỗi thời nhanh — thay bằng luật ở §SOP cập nhật của README: repo còn commit ≤3 tháng + issue tracker sống.)

## Paid Alternatives — luật nâng cấp (Open Source First của spec gốc)

| Tầng | Trả phí khi nào | Vì sao / ROI |
|---|---|---|
| LLM API | Local retry 2 lần vẫn dưới chuẩn ở bước cụ thể | Trả theo video, không theo tháng; trần $/video trong Charter; thường chỉ cần cho 1–2 bước khó nhất |
| TTS API (ElevenLabs-class) | Voice là lý do churn người xem (đo bằng retention đoạn nói) VÀ kênh đã có doanh thu | Khoảng cách với OSS đã hẹp [T2] — nâng cấp này ngày càng khó justify; thử Chatterbox/Qwen3-TTS trước |
| GPU cloud theo giờ | Cần video gen/render nặng <10 giờ/tuần | Thuê giờ < khấu hao GPU mua; mua GPU chỉ khi dùng >20h/tuần đều đặn 2 tháng |
| Stock footage trả phí | Ngách cần footage người/địa danh chất lượng mà nguồn free thiếu | Tính vào chi phí/video; thường rẻ hơn video gen về giờ công |

## Benchmark
Nguyên tắc của YAAS: **benchmark trên máy của mình là benchmark duy nhất có giá trị vận hành** — chạy smoke test §Workflow, ghi tok/s, giây-TTS/phút-audio, phút-render/phút-video vào DB. Benchmark ngoài chỉ dùng để CHỌN ứng viên (đã dẫn nguồn ở Decision Tree), không dùng để lập kế hoạch công suất.

## Cost
Khởi điểm $0 (điện + đĩa). Cấu hình 16GB RAM không GPU vận hành được toàn pipeline trừ video gen — đây là cấu hình tham chiếu của spec. Mỗi state ghi cost vào DB (`00`), kể cả cost=0, để khi bật fallback trả phí là thấy ngay tỷ trọng.

## ROI
Stack local ROI thể hiện ở: chi phí biên mỗi video ~0 → cho phép 20 video học nghề không áp lực tiền; và không phụ thuộc giá API của bên thứ ba (rủi ro đổi giá là failure mode có thật của factory chạy cloud 100%).

## Checklist
- [ ] `stack.yaml` đầy đủ các tầng theo format của kênh, version pin
- [ ] Smoke test end-to-end chạy xong, baseline ghi vào DB
- [ ] Mọi model production có license thương mại hợp lệ (kiểm từng cái, ghi vào yaml)
- [ ] Fallback trả phí khai báo trần $/video
- [ ] Đĩa trống ≥100GB, backup artifact cấu hình xong (`00` Recovery)

## Validation
Stack đạt khi: sản xuất được 1 video 60s test không lỗi; đỉnh RAM không chạm swap khi chạy tầng nặng nhất; đổi 1 model trong yaml không phải sửa code pipeline.

## Failure Modes & Recovery
1. **OOM/swap chết máy giữa batch** → chạy tuần tự từng tầng (không song song LLM+TTS trên 16GB); model nhỏ hơn 1 bậc quant; Recovery: state machine resume từ artifact (`00`).
2. **Model update làm đổi giọng/đổi hành vi** → version pin; update chỉ qua benchmark video chuẩn (`00` Failure #5).
3. **License đổi ở bản mới** (đã xảy ra nhiều lần trong hệ OSS) → pin version cũ vẫn dùng license cũ; rà quý theo README.
4. **TTS hallucination/đọc sai số & tên riêng** → QC bắt buộc bằng faster-whisper đối chiếu script (chi tiết `04`); từ điển phát âm cho thuật ngữ ngách.
5. **Driver GPU hỏng sau update OS** → môi trường Docker hoá cho tầng GPU; hoãn update OS giữa chu kỳ sản xuất.

## Automation
Toàn bộ install/smoke-test viết thành script `setup.sh` + `benchmark.py` trong repo — máy mới dựng lại stack <1 giờ.

## Prompt Templates
N/A (chapter hạ tầng). Prompt hệ thống cho LLM local (script/research) ở `03`.

## Optimization
Mỗi quý chạy lại 3 search: "best open source TTS/LLM/video generation <năm>" — lĩnh vực đổi nhanh (Qwen3-TTS mới 01/2026, LTX-2.x mới cuối 2025) `[T2]`; chỉ thay model khi benchmark video chuẩn thắng model đương nhiệm rõ ràng, không thay theo hype.

## Scaling
Scale sản lượng trước hết bằng batch đêm (máy chạy khi ngủ) → GPU thuê giờ → máy thứ hai/worker remote (queue của `00` đã thiết kế cho việc này) → xem `09`.

## Self Audit
1. *Khoảng trống?* Chưa benchmark tiếng Việt cho TTS (Kokoro hạn chế ngôn ngữ; Qwen3-TTS là ứng viên chính — cần test thật, `04` phải làm). License FLUX.1-dev và LTX bản mới cần VERIFY tại thời điểm dùng.
2. *OSS tốt hơn?* TTS: theo dõi TADA (Hume, 03/2026, claim zero-hallucination) — mới, chưa đủ track record `[T3→T2 đang hình thành]`.
3. *Benchmark mới hơn?* Nguồn đã dẫn là 2026-hiện-hành; quý sau chạy lại theo §Optimization.
4. *Đơn giản hơn?* Có: cấu hình "không GPU, không video gen, ảnh + FFmpeg" là bản đơn giản đủ tốt cho phần lớn ngách docu/education — đã đặt làm mặc định.
5. *Tự động hoá thêm?* setup.sh + benchmark.py đã là mức hợp lý; thêm nữa là gold-plating.

## Next Task
`03` — pipeline kịch bản: research → script → fact check → human gate, kèm prompt templates đủ 13 mục.
