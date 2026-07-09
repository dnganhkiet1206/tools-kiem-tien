# YAAS — YouTube Automation AI Specification

```yaml
version: 2.0.0
last_updated: 2026-07-09
governance: GOVERNANCE.md   # luật kiến trúc — đọc trước khi viết/sửa bất kỳ module nào
```

Specification kỹ thuật cho một **Content Factory YouTube** vận hành bởi một người, laptop, ngân sách khởi điểm ~0, ưu tiên AI mã nguồn mở. Viết machine-first theo chuẩn knowledge base để mọi AI (GPT, Claude, Gemini, Qwen, DeepSeek, Mistral, Llama...) và con người đọc và áp dụng nhất quán.

**Reader Baseline** (điều duy nhất spec được phép giả định — GOVERNANCE C2): người đọc/agent biết mở terminal, cài phần mềm theo hướng dẫn, chạy lệnh được cung cấp, có internet. Mọi tri thức còn lại spec tự dạy.

## Quan hệ với `ai-money-os/`

Hai tài liệu trả lời hai câu hỏi khác nhau và **không được đọc thay thế nhau**:

- `ai-money-os/01` trả lời **CÓ NÊN / KHI NÀO**: YouTube là *kênh*, không phải *mô hình*; faceless channel hàng loạt bị LOẠI làm nguồn thu chính; content chỉ nên bật từ Giai đoạn 1+ khi đã có dòng tiền.
- `yaas/` trả lời **NHƯ THẾ NÀO**: nếu quyết định vận hành kênh YouTube (làm kênh cho chính mình theo `ai-money-os/06`, hoặc **bán dịch vụ content factory cho khách** theo `ai-money-os/02` — đây là use case có xác suất doanh thu cao nhất của spec này), thì đây là bản thiết kế sản xuất.

Ràng buộc thiết kế số 1 của toàn spec, rút từ chính sách YouTube (xem `01`): **hệ thống này tự động hoá SẢN XUẤT, không tự động hoá GIÁ TRỊ** — mọi pipeline đều có human-in-the-loop gate, vì content mass-produced không biến thể đã bị YouTube loại khỏi monetization từ 07/2025.

## Tuyên bố trung thực & Evidence Policy (áp dụng toàn spec)

1. Tài liệu do AI tổng hợp; tác giả không phải là "operator đã kiếm doanh thu thực" — mọi claim doanh thu trên mạng về YouTube automation phần lớn là Tier 3 không kiểm chứng được, và spec này đánh dấu chúng đúng như vậy.
2. Mọi khẳng định gắn nhãn nguồn theo GOVERNANCE PHASE 7: `[A]` official docs/paper/benchmark chính thức · `[B]` production case study đã xác minh/OSS docs/conference · `[C]` community benchmark/practitioner reports · `INSUFFICIENT EVIDENCE` khi không đủ nguồn · `VERIFY` khi số liệu có thể đã cũ (knowledge cutoff của AI viết spec: 01/2026, có kiểm chứng web 07/2026). *(File viết trước v2 còn dùng nhãn cũ T1/T2/T3 — ánh xạ: T1→A, T2→B, T3→C; sẽ đổi khi migrate theo GOVERNANCE i4.)*
3. Lĩnh vực này đổi theo tháng. Mỗi chapter có mục **Self Audit** và spec có SOP cập nhật (cuối file này). Model/tool cụ thể sẽ lỗi thời — **cấu trúc quyết định thì không**: mọi đề xuất tool đều kèm decision tree để thay tool mà không đổi kiến trúc.

## Mục lục

| File | Nội dung | Trạng thái |
|---|---|---|
| `GOVERNANCE.md` | PHASE 1–9: xung đột đã giải, giả định, sổ evidence, kiến trúc spec, DoD | ✅ v2 |
| `schemas/` | module.schema.yaml · tool.schema.yaml · prompt.schema.yaml (bản chuẩn) | ✅ v2 |
| `exports/state-machine.yaml` | State machine máy-đọc (canonical — thắng prose khi lệch) | ✅ v2 |
| `00-kien-truc-tong-the.md` | Kiến trúc Content Factory, State Machine 20 trạng thái, nguyên tắc thiết kế | ✅ v2 |
| `01-chien-luoc-kenh-monetization.md` | Chọn ngách/format, chính sách YPP & inauthentic content, kinh tế học của kênh | ✅ |
| `02-stack-phan-cung-local-ai.md` | Decision tree phần cứng, LLM/TTS/STT/Image/Video local, bảng đánh giá tool | ✅ |
| `03-pipeline-kich-ban.md` | Research → Script → Fact Check → Review + prompt templates | 🔜 |
| `04-pipeline-giong-noi.md` | TTS production: chọn model, xử lý lỗi phát âm, QC audio | 🔜 |
| `05-pipeline-hinh-anh-video.md` | Image gen, stock footage, video assembly bằng FFmpeg/editor | 🔜 |
| `06-thumbnail-seo-publishing.md` | Thumbnail, title/description/tags, YouTube Data API, lịch đăng | 🔜 |
| `07-orchestration-n8n-state-machine.md` | Hiện thực hoá state machine bằng n8n + Python + queue + retry | 🔜 |
| `08-analytics-optimization.md` | Vòng phản hồi số liệu → cải tiến; KPI đầy đủ | 🔜 |
| `09-scaling-da-kenh.md` | Nhân kênh, đa ngôn ngữ, thuê người/agent | 🔜 |

## SOP cập nhật spec (chạy mỗi quý)

1. Với mỗi tool được đề xuất: kiểm tra repo còn maintain (commit ≤3 tháng), license chưa đổi, có bản thay thế mạnh hơn chưa (search "best open source X <năm hiện tại>").
2. Kiểm tra thay đổi chính sách YouTube: [YouTube channel monetization policies](https://support.google.com/youtube/answer/1311392) — đây là dependency rủi ro cao nhất của toàn hệ.
3. Chạy 5 câu Self Audit cuối mỗi chapter; cập nhật tại chỗ, ghi ngày vào changelog chapter.
