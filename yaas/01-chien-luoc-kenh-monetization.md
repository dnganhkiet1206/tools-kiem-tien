# YAAS 01 — Chiến lược kênh, chính sách & kinh tế học monetization

## Goal
Chốt 3 quyết định trước khi chạm vào công cụ: (1) mô hình doanh thu của kênh, (2) ngách + format, (3) ranh giới chính sách không được vượt. Sai ở chapter này thì mọi chapter sau chỉ là tự động hoá một thứ chết từ đầu.

## Background — sự thật chính sách phải thiết kế quanh nó

**07/2025, YouTube đổi chính sách "repetitious content" thành "inauthentic content"** trong YouTube Partner Program: content mass-produced hoặc lặp template với biến thể tối thiểu **không đủ điều kiện kiếm tiền**; content AI không bị cấm, nhưng phải có giá trị gốc (original value) và bật disclosure "altered or synthetic" khi áp dụng `[T1: YouTube Help — channel monetization policies]` `[T2: đưa tin và phân tích đồng thuận nhiều nguồn]`. Một số nguồn Tier 3 mô tả cơ chế xử phạt 3 nấc (cảnh cáo → treo 90 ngày → loại vĩnh viễn khỏi YPP) — `[T3, VERIFY trực tiếp trong YouTube Help trước khi dựa vào]`.

**Hệ quả thiết kế cho toàn YAAS:** "YouTube automation" kiểu 2022–2023 (TTS đọc script generic trên slideshow stock, xả hàng loạt) hiện là mô hình **bị chính nền tảng loại trừ**, trùng khớp với phán quyết kinh doanh ở `ai-money-os/01`. Spec này vì thế thiết kế factory cho **kênh ngách có giá trị gốc**: nghiên cứu thật, góc nhìn thật, human gate — máy móc hoá phần sản xuất, không máy móc hoá phần giá trị.

**Điều kiện vào YPP:** 1.000 subscribers + 4.000 giờ xem công khai 12 tháng, HOẶC 1.000 subscribers + 10 triệu view Shorts 90 ngày `[T1 — VERIFY ngưỡng hiện hành, YouTube đã từng thêm bậc trung gian cho fan funding]`.

## Theory — kinh tế học của một kênh

```
Doanh thu kênh = Σ (Views × RPM_ads) + doanh thu ngoài-ads (affiliate, sản phẩm, dịch vụ, tài trợ)
Lợi nhuận     = Doanh thu − (giờ người × giá giờ) − chi phí tool/API − chi phí cơ hội
```

3 định luật rút ra (mức tin cậy: suy luận từ cấu trúc, không phải thống kê):
1. **RPM do NGÁCH và NGƯỜI XEM quyết định, không do volume:** ngách business/finance/tech tiếng Anh thường được cộng đồng báo cáo RPM cao hơn nhiều lần ngách giải trí đại chúng `[T3 — số cụ thể INSUFFICIENT EVIDENCE, dao động quá lớn theo quốc gia người xem/mùa/kênh; không thiết kế kế hoạch tài chính dựa trên RPM đồn đại]`.
2. **Ads là nguồn thu YẾU nhất của kênh nhỏ:** với <100k views/tháng, doanh thu ads thường không đủ trả giờ công. Kênh solo có lãi sớm là kênh dùng views để bán thứ khác (dịch vụ `ai-money-os/02`, affiliate ngách, sản phẩm số `ai-money-os/10`). YPP là mốc phụ, không phải business model.
3. **Chi phí thật của factory là GIỜ NGƯỜI ở 2 human gate + planning** — automation giảm giờ sản xuất, không giảm giờ phán đoán; vì thế Production Time/video và Automation Rate là KPI chi phí quan trọng nhất (xem §Metrics).

## Architecture (của quyết định chiến lược)
Chuỗi quyết định: Mô hình doanh thu → Ngách → Format → Tần suất → chỉ khi cả 4 chốt mới sang `02`.

## Workflow / Decision Tree

```
D1. MÔ HÌNH DOANH THU
IF bạn có dịch vụ/sản phẩm sẵn (đang chạy ai-money-os G1+)
THEN kênh = cỗ máy inbound: đề tài chọn theo nỗi đau ICP, CTA về offer; RPM không quan trọng
     → đây là cấu hình ROI cao nhất và được khuyến nghị mặc định
ELSE-IF bạn bán content factory làm DỊCH VỤ cho khách có kênh
THEN toàn YAAS là delivery SOP của bạn; khách chịu rủi ro kênh — cấu hình an toàn thứ hai
ELSE (kênh độc lập sống bằng ads/affiliate)
THEN chấp nhận: 6–18 tháng không lương, xác suất kiểu xổ số (`ai-money-os/01` đã cảnh báo) —
     chỉ làm khi có nguồn sống khác; bắt buộc thiết kế doanh thu ngoài-ads từ video 1

D2. NGÁCH — chấm 0–2 điểm mỗi tiêu chí, nhận ngách ≥8/12:
     [Bạn có hiểu biết/quan tâm thật đủ 100 video]  [Giá trị gốc tạo được bằng research sâu]
     [Người xem có ý định thương mại (bán được gì đó)]  [Đề tài thường xanh (evergreen), ít phụ thuộc trend]
     [Sản xuất được không lộ mặt mà vẫn KHÔNG generic]  [Cạnh tranh top 10 kết quả còn kẽ hở chất lượng]

D3. FORMAT
IF ngách nặng giải thích/câu chuyện (docu, phân tích, lịch sử, tài chính)
THEN long-form 8–15 phút, voice-over + visual minh hoạ — format hợp automation nhất
     và AVD/retention là chỉ số sống còn
ELSE-IF ngách demo/hướng dẫn ngắn
THEN long-form ngắn 4–8 phút hoặc hybrid + Shorts cắt từ long-form (Shorts thuần: RPM rất thấp,
     chỉ dùng làm kênh khám phá, không làm nguồn thu [T3])
KHÔNG chọn format đòi quay người thật nếu mục tiêu là factory (mâu thuẫn kiến trúc)

D4. TẦN SUẤT
Bắt đầu 1 video/tuần chất lượng > 3 video/tuần trung bình (chính sách inauthentic phạt
mass-production; thuật toán thưởng retention, không thưởng volume [T1/T2]) ·
chỉ tăng nhịp khi Automation Rate >70% VÀ số liệu 10 video đầu xác nhận format
```

## Input
Kết quả `ai-money-os/01` (nếu dùng chung hệ) hoặc bảng chấm ngách D2; 20 kênh đối chứng trong ngách dự kiến.

## Output
1 trang Channel Charter: mô hình doanh thu, ngách, ICP người xem, format, tần suất, persona kênh, ranh giới chính sách, trần chi phí/video, KPI mục tiêu 90 ngày.

## Dependencies
Chính sách YPP (dependency rủi ro #1, rà mỗi quý theo README) · thị trường ngách.

## Failure Modes
1. **Chọn ngách theo RPM đồn đại** → làm 30 video không hiểu về đề tài, giá trị gốc = 0, chết chính sách lẫn thuật toán. Counter: tiêu chí 1 của D2 là cứng.
2. **Đo thành công bằng view thay vì bằng mô hình D1** → kênh 100k view/tháng vẫn lỗ giờ. Counter: KPI doanh thu gắn từ Channel Charter.
3. **Nhái kênh đang thắng 1:1** → vào đúng vùng "easily replicable" của chính sách + không bao giờ vượt bản gốc. Counter: D2 tiêu chí "kẽ hở chất lượng" — phải nói được top 10 đang THIẾU gì.
4. **Đổi ngách sau 10 video vì chưa nổ** → 10 video là quá ít dữ liệu cho thuật toán lẫn cho chính mình. Ngưỡng đánh giá: 20–30 video cùng format (xem §Validation).

## Recovery Strategy
Kênh 25+ video không tín hiệu (CTR <2% và AVD <30% kéo dài): chẩn đoán theo thứ tự packaging (thumbnail+title — sửa rẻ nhất) → hook 30s đầu → đề tài (so search volume) → ngách. Đổi từng tầng như `ai-money-os/04` Recovery, mỗi lần 5 video. Kênh dính strike inauthentic: dừng đăng, audit toàn bộ template theo checklist §QA, kháng nghị chỉ sau khi đã sửa gốc.

## Automation
Chapter này KHÔNG tự động hoá — chọn ngách, charter, ranh giới chính sách là phán đoán người (đúng `ai-money-os/00` §3). AI hỗ trợ: gom dữ liệu 20 kênh đối chứng, phân tích khoảng trống đề tài.

## Prompt Templates
`prompts/strategy/nghien-cuu-nganh.md` — Role: analyst; Mission: lập bản đồ 20 kênh đối chứng (subs, tần suất, format, đề tài top, khoảng trống); Inputs: tên ngách; Outputs: bảng + 10 khoảng trống đề tài; Constraints: chỉ dữ liệu quan sát được, không suy diễn doanh thu; Validation: mỗi khoảng trống kèm bằng chứng (đề tài có search, top 10 chưa phủ); chi tiết đủ 13 mục khi hiện thực ở `03`.

## Open Source / Paid Alternatives
Nghiên cứu ngách: xem tay + YouTube Data API (free quota) đủ cho solo; tool trả phí (vidIQ/TubeBuddy…) chỉ đáng khi vận hành nhiều kênh — theo luật ROI của README `[T3 về hiệu quả các tool này]`.

## Benchmark
Chuẩn tham chiếu cộng đồng (Tier 3, dùng làm mốc thô, không phải mục tiêu): CTR 2–10% tuỳ ngách; retention 30s đầu >70% là hook đạt; AVD >40% với video 8–12 phút là tốt. `INSUFFICIENT EVIDENCE` cho chuẩn chính thức — YouTube không công bố ngưỡng.

## Cost
Chapter này: $0. Trần chi phí sản xuất khuyến nghị ghi vào Charter: giai đoạn học (video 1–20) chỉ tính bằng giờ người; đặt trần giờ/video (vd 6h) và ép giảm dần bằng `08`.

## ROI
ROI của chapter = tránh được chi phí lớn nhất toàn dự án: 6 tháng tự động hoá một kênh không bao giờ có khả năng ra tiền.

## Checklist
- [ ] D1 chọn xong mô hình doanh thu, viết được 1 câu "kênh này kiếm tiền bằng ___"
- [ ] Ngách đạt ≥8/12, có bảng chấm lưu lại
- [ ] 20 kênh đối chứng đã phân tích, chỉ ra được ≥5 khoảng trống chất lượng
- [ ] Channel Charter 1 trang hoàn thành, có ranh giới chính sách + disclosure plan
- [ ] Đọc trực tiếp trang chính sách monetization của YouTube (không đọc qua blog thuật lại)

## Validation
Charter đạt nếu: người ngoài đọc xong mô tả đúng kênh sẽ trông như thế nào; 3 video giả định đầu tiên đều trả lời được "giá trị gốc so với top 10 là gì?". Ngưỡng dữ liệu: mọi kết luận về format chỉ hợp lệ sau ≥20 video cùng cấu hình.

## Optimization
Charter là tài liệu sống: sửa mỗi quý theo `08`, nhưng KHÔNG sửa trong 20 video đầu (đổi biến giữa chừng phá dữ liệu — `ai-money-os/00` §5).

## Scaling
Charter thứ hai (kênh/ngôn ngữ mới) chỉ mở theo trigger `09`.

## Self Audit
1. *Khoảng trống?* Ngưỡng YPP và cơ chế strike cần VERIFY trực tiếp định kỳ; RPM theo ngách cố tình để INSUFFICIENT EVIDENCE — đúng hơn là giả chính xác.
2. *OSS tốt hơn?* N/A (chapter chiến lược).
3. *Benchmark mới?* Chuẩn CTR/AVD là consensus Tier 3 — nếu tìm được nghiên cứu Tier 1–2 thì thay.
4. *Đơn giản hơn?* D1 mặc định "kênh phục vụ offer sẵn có" chính là đường đơn giản nhất — đã đặt làm khuyến nghị.
5. *Tự động hoá thêm?* Gom số liệu kênh đối chứng tự động hoá được bằng YouTube API — đưa vào `08`.

## Next Task
`02` — chọn stack phần cứng + local AI khớp với format đã chốt (format quyết định stack, không ngược lại).
