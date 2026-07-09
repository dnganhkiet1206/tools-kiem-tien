# YAAS GOVERNANCE — Kết quả PHASE 1–9 (bắt buộc đọc trước khi viết/sửa bất kỳ module nào)

```yaml
version: 2.0.0
last_updated: 2026-07-09
status: STABLE            # kiến trúc đã qua PHASE 8-9, được phép viết module (PHASE 10)
supersedes: yaas v1 (batch 00-02 viết trước governance — xem MIGRATION cuối file)
```

---

## PHASE 1 — Phân tích yêu cầu

Yêu cầu gốc phân rã thành 6 nhóm ràng buộc:

| # | Nhóm | Nội dung cốt lõi | Kiểm chứng được bằng |
|---|---|---|---|
| R1 | Đối tượng | Solo operator, laptop, chi phí ~0, mở rộng dần thành factory | Mọi decision tree có nhánh "budget=0 / RAM thấp" |
| R2 | Công nghệ | Open source first; trả phí chỉ khi ROI chứng minh | Mỗi tool có Open Source Equivalent + luật nâng cấp |
| R3 | Tri thức | Evidence A/B/C; không suy đoán; VERIFY/INSUFFICIENT EVIDENCE | Nhãn trên mọi claim định lượng |
| R4 | Hình thức | Machine-first, schema hoá, self-contained module, đa định dạng export | `schemas/` + `exports/` + Definition of Done |
| R5 | Vận hành | Decision tree thay cho "dùng X"; QC sau mỗi chapter; versioning | Module header + Self Audit bắt buộc |
| R6 | Mục tiêu cuối | Tài liệu đứng một mình, đủ để dựng lại toàn hệ khi hội thoại bị xoá | Bài test "cold reader" trong PHASE 7 |

## PHASE 2 — Xung đột & cách giải (quyết định kiến trúc, có hiệu lực như luật)

**C1. "Mỗi module tự đầy đủ" ⟂ "Không filler, không lặp".**
Tự-đầy-đủ tuyệt đối × 10 module = lặp ~40% nội dung. **Giải:** mẫu **Context Capsule** — mỗi module mở đầu bằng ≤15 dòng nén toàn bộ ngữ cảnh tối thiểu (hệ là gì, hiến pháp rút gọn, vị trí module, thuật ngữ dùng trong module). Tham chiếu sang module khác chỉ được phép ở mục Alternatives/Dependencies và luôn kèm 1 câu tóm tắt nội dung được tham chiếu — người chỉ có 1 file vẫn hành động được, người có cả repo không phải đọc trùng.

**C2. "Không giả định người đọc biết gì ngoài spec" ⟂ độ dài hữu hạn.**
Hiểu theo nghĩa đen → phải dạy cả cách dùng bàn phím; hồi quy vô hạn. **Giải:** khai báo **Reader Baseline** tường minh (điều duy nhất spec được phép giả định): người đọc (hoặc AI agent) biết mở terminal, cài phần mềm theo hướng dẫn, chạy lệnh được cung cấp, và có internet. Mọi tri thức domain (YouTube, AI stack, pipeline) spec phải tự dạy. Deviation này được ghi nhận công khai tại đây thay vì giấu.

**C3. "Automation over Manual Work" ⟂ "Human Review over Blind Trust".**
**Giải (đã là kiến trúc):** tự động hoá *sản xuất*, người gác *giá trị* — 2 human gate cố định (duyệt script sau fact-check, duyệt video trước publish). Trần automation thiết kế ~85%, không phải 100%; con số này là hệ quả của hiến pháp, không phải hạn chế kỹ thuật.

**C4. "Mọi kết luận cần bằng chứng" ⟂ lĩnh vực đổi theo tháng (benchmark chết nhanh).**
**Giải:** (a) benchmark ngoài chỉ để *chọn ứng viên*, benchmark trên máy của chính operator là chuẩn *vận hành*; (b) mọi claim định lượng mang nhãn + ngày; (c) trường `future_review` bắt buộc trong header mỗi module; (d) SOP rà quý.

**C5. "Machine first" ⟂ người vẫn phải vận hành được.**
**Giải:** bản chuẩn (canonical) của dữ liệu có cấu trúc là YAML/JSON trong `exports/`; prose trong module là bản diễn giải. Khi hai bản lệch nhau, exports thắng và prose phải sửa theo.

**C6. "Open Source over Vendor Lock-in" ⟂ toàn hệ đứng trên một nền tảng đóng (YouTube).**
Không giải được ở tầng kỹ thuật — đây là **rủi ro nền tảng không khử được**, chỉ giảm nhẹ: artifact + metadata sở hữu local 100% (tái đăng nền tảng khác được), và tầng kinh doanh phải rút người xem về tài sản sở hữu (email — thuộc phạm vi `ai-money-os`, ngoài scope YAAS). Ghi nhận trung thực thay vì giả vờ đã giải.

**C7. Chi phí ~0 ⟂ chuẩn production-grade (backup, dự phòng, QC).**
**Giải:** ba bậc trưởng thành vận hành (ops maturity): M1 tay+checklist (video 1–5) → M2 bán tự động (6–15) → M3 factory (16+); mỗi module ghi rõ yêu cầu nào bắt buộc từ bậc nào. Bắt solo operator tuân chuẩn M3 từ ngày 1 là cách chắc nhất để spec bị vứt xó.

## PHASE 3 — Giả định đang tồn tại (công khai để ai cũng kiểm được)

| # | Giả định | Rủi ro nếu sai | Giám sát |
|---|---|---|---|
| A1 | Operator có 10–20h/tuần | Lịch sản xuất trong module vô nghĩa | Charter cá nhân hoá lại nhịp |
| A2 | Laptop ≥8GB RAM, đĩa ≥100GB trống | Stack local không chạy | Decision tree đã có nhánh fallback API |
| A3 | YouTube giữ vị thế + API ổn định trong 2–3 năm | Toàn spec mất đối tượng | C6; rà chính sách quý |
| A4 | Model mở tiếp tục bám sát model đóng | Luật "open first" mất hiệu lực ở tầng nào đó | Search định kỳ theo SOP quý |
| A5 | Ngách/format đã chọn xong ở tầng kinh doanh | Factory tự động hoá một kênh chết | Module 01 chặn: chưa có Charter thì không sang 02 |
| A6 | Prose tiếng Việt + thuật ngữ Anh đọc được bởi cả người lẫn AI đa model | Interop giảm | Exports YAML dùng key tiếng Anh 100% |

## PHASE 4 — Thiếu bằng chứng (sổ đăng ký trung tâm — module chỉ trỏ về đây)

| ID | Claim | Trạng thái | Hành động |
|---|---|---|---|
| E1 | RPM theo ngách (con số cụ thể) | INSUFFICIENT EVIDENCE — biến thiên quá lớn | Không đưa số vào kế hoạch tài chính |
| E2 | Ngưỡng YPP hiện hành + cơ chế strike inauthentic 3 nấc | VERIFY (nguồn C mô tả, cần đọc trực tiếp YouTube Help) | Rà quý, nguồn Level A duy nhất là support.google.com |
| E3 | License Qwen3-TTS, LTX bản mới, FLUX.1-dev khi dùng thương mại | VERIFY tại thời điểm dùng | Checklist module 02/04/05 |
| E4 | Quota YouTube Data API (10k units, upload ~1.600) | VERIFY | Module 06 |
| E5 | Chất lượng TTS tiếng Việt của các model mở | Chưa có test — phải benchmark tay | Module 04 bắt buộc test trước khi chọn |
| E6 | Ảnh hưởng của disclosure "altered/synthetic" lên reach | INSUFFICIENT EVIDENCE | Không thiết kế quanh suy đoán; disclosure bật theo luật |
| E7 | Claim doanh thu của "YouTube automation" creators | Level C không kiểm chứng — mặc định KHÔNG TIN | Đã ghi vào README |

## PHASE 5 — Kiến trúc Specification

```
yaas/
├── GOVERNANCE.md          ← file này: luật, xung đột đã giải, sổ evidence, migration
├── README.md              ← cổng vào: mục lục, trạng thái, SOP cập nhật quý
├── schemas/               ← CANONICAL: module.schema.yaml, tool.schema.yaml, prompt.schema.yaml
├── exports/               ← dữ liệu máy-đọc: state-machine.yaml, (mỗi module thêm dần)
├── prompts/               ← prompt production, mỗi file tuân prompt.schema
└── NN-<ten-module>.md     ← module 00..09, mỗi file tuân module.schema
```

Nguyên tắc phụ thuộc: module → schemas + exports (được phép); module → module khác (chỉ qua Dependencies/Alternatives kèm tóm tắt 1 câu, theo C1); GOVERNANCE không phụ thuộc gì.

## PHASE 6 — Schema (bản chuẩn nằm ở `schemas/`, đây là tóm tắt)

- **module.schema.yaml** — header versioning 6 trường (version, last_updated, evidence_level tổng, confidence, risk, future_review) + Context Capsule + 16 mục bắt buộc (Purpose → ROI) + Self Audit 8 câu + Exports liệt kê định dạng đã xuất.
- **tool.schema.yaml** — 22 trường đánh giá tool (Purpose → Scalability), thêm `evaluated_at` vì đánh giá tool mục nát nhanh nhất.
- **prompt.schema.yaml** — 12 khối (ROLE → SUCCESS CRITERIA) + header versioning.

## PHASE 7 — Tiêu chuẩn đánh giá

**Definition of Done cho một module (đủ 6 mới được merge):**
1. Đúng schema, đủ 16 mục, header versioning đầy đủ.
2. Cold-reader test: một AI khác (hoặc người) chỉ đọc file đó + Reader Baseline, thực thi được Execution không phải hỏi thêm.
3. Mọi claim định lượng có nhãn A/B/C/VERIFY/IE, có ngày.
4. Mọi lựa chọn công nghệ là decision tree có trade-off + fallback, không có câu "dùng X" trần.
5. Có ít nhất 1 export máy-đọc trong `exports/` (YAML/JSON/Mermaid) và prose không mâu thuẫn với export (C5).
6. Self Audit 8 câu trả lời thật (có ít nhất 1 điểm yếu được thừa nhận — module "không có điểm yếu" là module chưa được audit).

**Thang Confidence (trong header):** HIGH = nhiều nguồn A/B đồng thuận + tự kiểm chứng được; MEDIUM = nguồn B/C hoặc suy luận cấu trúc chặt; LOW = nguồn C đơn lẻ / lĩnh vực đổi nhanh — bắt buộc future_review ≤ 3 tháng.
**Thang Automation Score (0–5):** 0 = người làm toàn bộ … 3 = máy làm, người duyệt … 5 = máy làm + tự validate, người chỉ xem báo cáo.
**Nhãn bằng chứng:** LEVEL A/B/C theo directive; ánh xạ từ v1: T1→A, T2→B, T3→C (giữ nguyên ngữ nghĩa).

## PHASE 8 — Tự phản biện (điểm yếu của chính kiến trúc này)

1. **Nguy cơ quan liêu:** schema 16 mục + 22 trường tool cho MỘT người vận hành có thể nặng hơn chính công việc. → Giải ở PHASE 9-i1.
2. **Self-contained vẫn là thoả hiệp:** Context Capsule 15 dòng không thể chứa hết; cold-reader của 1 file lẻ vẫn thiệt so với người có cả repo. Chấp nhận có chủ đích (C1), không giả vờ đã giải tuyệt đối.
3. **Spec viết bởi AI có knowledge cutoff:** dù có web-verify, mọi đánh giá tool là ảnh chụp 07/2026 — giá trị bền của spec nằm ở decision tree + schema + SOP cập nhật, KHÔNG nằm ở tên model. Người dùng spec sau 6 tháng phải chạy SOP quý trước khi tin bảng tool.
4. **Chưa có bộ test tự động cho chính spec:** Definition of Done đang kiểm bằng tay. → PHASE 9-i2.
5. **Rủi ro đạo đức còn lại:** spec có thể bị dùng để chạy content farm bất chấp human gate. Giảm nhẹ: human gate là điều kiện Validation của state machine (bỏ gate = pipeline không đạt DoD), và ràng buộc chính sách inauthentic được đặt làm ràng buộc thiết kế số 1 — nhưng spec không thể ép người dùng; ghi nhận giới hạn.

## PHASE 9 — Cải tiến đã áp dụng

- **i1 (chống quan liêu):** thêm cột `required_from` (M1/M2/M3) vào schema — bậc M1 chỉ bắt buộc ~1/3 số mục (Purpose, Inputs, Outputs, Execution, Validation, Failure Modes); phần còn lại bắt buộc từ M2/M3. Solo operator ngày 1 dùng được spec mà không chết chìm.
- **i2 (spec tự kiểm):** `exports/` dùng YAML thuần → viết được linter 50 dòng Python kiểm schema-compliance (đưa vào module 07 như một task của orchestration repo).
- **i3 (đa model):** mọi prompt template cấm tính năng riêng của một vendor; chỗ khác biệt giữa model (system prompt format, tool-call syntax) tách vào 1 adapter note duy nhất trong module 03.
- **i4 (migration có trật tự):** không đập đi viết lại batch v1 — nâng cấp tại chỗ theo bảng MIGRATION dưới, mỗi lần chạm module nào thì nâng module đó lên v2 schema.

## PHASE 10 — Trạng thái: ĐƯỢC PHÉP VIẾT

Kiến trúc khoá tại version 2.0.0. Mọi module mới viết theo `schemas/`; thay đổi kiến trúc phải sửa file này trước, tăng version, ghi lý do.

## MIGRATION (v1 → v2)

| File v1 | Việc cần làm | Trạng thái |
|---|---|---|
| README.md | Thêm Reader Baseline, đổi nhãn T→A/B/C, trỏ về GOVERNANCE | ✅ đã nâng |
| 00-kien-truc-tong-the.md | Thêm header versioning + Context Capsule + export state-machine.yaml + Mermaid | ✅ đã nâng |
| 01, 02 | Nâng cùng cấu trúc khi chạm lần tới (i4) | 🔜 |
| 03–09 | Viết mới thẳng theo v2 | 🔜 |
