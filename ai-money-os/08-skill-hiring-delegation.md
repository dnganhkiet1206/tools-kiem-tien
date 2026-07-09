# 08 — SKILL: Hiring, Delegation & Agency hoá

Thoát trần thời gian của founder — rủi ro nhất trong các skill của OS: thuê sai người đốt tiền nhanh hơn mọi lỗi khác, và uỷ quyền sai cách làm mất khách đã khó nhọc kiếm được. Skill này quyết định `02` dừng ở "việc tự do tự làm" hay lớn thành doanh nghiệp.

## 1. Tên Skill
Xây bộ máy con người + AI thay dần founder trong giao hàng và vận hành, theo đúng trật tự và đúng trigger — không sớm hơn, không muộn hơn.

## 2. Mục tiêu
Mỗi quý giảm % giờ founder trong giao hàng mà chất lượng (đo bằng QC pass rate và churn) không giảm; đích G2→G3 (`05`): founder ≤30% giờ giao hàng, rồi tiến tới bài kiểm tra nghỉ-2-tuần.

## 3. Khi nào dùng
- Chạm trigger thuê của skill gốc (`02` §14, `03` §14, `06` §14, `07` §14) — file này là phần mở rộng chung của các trigger đó.
- Doanh thu đã ổn định đủ trả người 3 tháng mà không cần deal mới nào (quy tắc runway thuê).

## 4. Khi nào KHÔNG dùng
- Thuê để chạy trốn hỗn loạn ("việc ngập đầu quá") khi chưa có SOP → thuê người vào hỗn loạn tạo ra hỗn loạn đắt tiền hơn. SOP trước, người sau — không ngoại lệ.
- Thuê vì hình ảnh ("có team cho ra dáng công ty") → chi phí cố định là kẻ giết doanh nghiệp nhỏ số một.
- Doanh thu chưa đủ quy tắc runway 3 tháng → chưa đủ, kể cả đang quá tải (giải bằng tăng giá `02` §13 trước — tăng giá là cách giảm tải không tốn lương).

## 5. Framework ra quyết định — thuê ai, theo trật tự nào

```
TRẬT TỰ THUÊ CHUẨN (vi phạm trật tự = nguồn thất bại phổ biến nhất của skill này):
  1. VA / freelancer part-time      → việc SOP-hoá, lặp lại, sai ít hại (research, list, admin, đóng gói)
  2. Freelancer giao hàng           → bước giao hàng đã có SOP chạy ≥10 lần
  3. Delivery lead / account manager → khi có ≥8–10 khách active, giữ quan hệ + kiểm chất lượng
  4. Sales                          → CUỐI CÙNG, chỉ sau khi founder tự chốt ≥15 deal
                                      và viết xong playbook bán (kịch bản, xử lý từ chối, giá sàn)

IF một việc cần thuê
THEN hỏi theo thứ tự:
     (a) Bỏ được không? (nhiều việc "phải làm" thật ra bỏ được — rẻ nhất)
     (b) Tự động hoá/AI agent được không? (xem §15 — rẻ nhì, không lương tháng)
     (c) SOP-hoá rồi giao người rẻ theo giờ/sản phẩm được không?
     (d) Cần người có phán đoán → thuê đắt, phỏng vấn kỹ, thử việc có số đo
     Chỉ xuống bậc dưới khi bậc trên trả lời KHÔNG.

IF phân vân giữa AI agent và người cho một việc
THEN chấm: việc số hoá được? input/output chuẩn? lỗi chịu được (có người kiểm mẫu)? khối lượng đều?
     → 4/4 CÓ: agent. Có "quan hệ con người" hoặc "phán đoán ngữ cảnh mới" trong việc: người.
     → Lai (phổ biến nhất): agent làm 70% thô, người 30% tinh + chịu trách nhiệm cuối.

IF khách phàn nàn chất lượng sau khi giao việc cho người mới
THEN founder nhận lại KHÁCH ĐÓ ngay (giữ quan hệ), KHÔNG nhận lại cả loại việc đó;
     sửa SOP hoặc thay người — quy về hệ thống, không quy về "thôi mình tự làm cho lành"
     (câu đó là cách founder tự nhốt mình vĩnh viễn ở G1)
```

## 6. SOP từng bước — một lần thuê chuẩn

```
INPUT        1 việc chạm trigger + SOP của việc đó đã chạy tay ≥10 lần + runway 3 tháng
   ↓
RESEARCH     Viết scorecard 1 trang: kết quả kỳ vọng đo được (không phải mô tả tính cách) ·
             mức giá thị trường cho việc này (hỏi AI + 2 nguồn tuyển thật) · nơi tìm:
             freelancer platform / cộng đồng ngách / referral từ mạng lưới đã có
   ↓
DECISION     Chọn cấu trúc trả: theo giờ (việc chưa chuẩn) → theo sản phẩm/deliverable
             (việc đã productize — ưu tiên, tự khớp incentive với `02`) → lương+thưởng giữ khách
             (vai account manager trở lên)
   ↓
EXECUTION    TRIAL TASK CÓ TRẢ TIỀN, nhỏ, thật, chấm bằng scorecard — cho 2–3 ứng viên song song ·
             chọn 1 → thử việc 2–4 tuần với 3 số đo công bố trước (chất lượng QC, đúng hạn, độ tự chủ)
   ↓
MEASUREMENT  QC pass rate (founder kiểm mẫu 100% tuần 1 → 50% tuần 2–3 → 10–20% khi ổn định) ·
             giờ founder tiết kiệm thật/tuần · phản hồi khách với deliverable của người mới
   ↓
OPTIMIZATION Mỗi lỗi của người thuê = 1 dòng cập nhật SOP (lỗi lặp là lỗi của SOP, không phải của người) ·
             1-1 ngắn hằng tuần: xem số, gỡ kẹt, KHÔNG micro-manage từng thao tác
   ↓
AUTOMATION   Ghép agent hỗ trợ người thuê (agent làm thô, người tinh) — năng suất 1 người
             tăng 2–3 lần trước khi cần người thứ hai
   ↓
SCALE        Người đầu ổn định (QC >90% liên tục 4 tuần) → người đó đào tạo người sau bằng chính SOP
             → founder chỉ còn tuyển + giữ chuẩn · lên nấc 3 (account manager) khi ≥8–10 khách active
```

### 6b. SOP theo nhịp
**Daily (founder, giai đoạn có team):** kiểm mẫu QC theo tỷ lệ hiện hành · gỡ kẹt trong ngày cho người đang chờ quyết định (người chờ founder = tiền đứng im).
**Weekly:** 1-1 15–30'/người: 3 số đo + 1 kẹt + 1 cải tiến SOP · rà tổng giờ founder trong giao hàng (con số phải giảm theo quý).
**Monthly:** chi phí người/doanh thu (trần lành mạnh giai đoạn G2: ≤35–40%) · rà ai vượt chuẩn (tăng phạm vi + tiền) và ai dưới chuẩn 2 tháng liền (§9).
**Quarterly:** bài kiểm tra vắng mặt: founder rút khỏi vận hành 3–5 ngày, ghi lại mọi thứ gãy → mỗi thứ gãy = 1 SOP/1 uỷ quyền còn thiếu → đó là danh sách việc quý sau.

## 7. Checklist trước mỗi lần thuê
- [ ] Việc đã qua chuỗi (a)(b)(c)(d) ở §5 — có lý do rõ vì sao phải là người, ở bậc này
- [ ] SOP việc đó chạy tay ≥10 lần, người ngoài đọc hiểu ~70%
- [ ] Runway 3 tháng lương từ doanh thu hiện tại
- [ ] Scorecard kết quả đo được + 3 số đo thử việc công bố trước
- [ ] Trial task trả tiền thiết kế xong
- [ ] Đúng trật tự thuê chuẩn (không thuê sales khi chưa đủ 15 deal tự chốt)

## 8. Sai lầm phổ biến
1. Thuê sales đầu tiên "để mình khỏi phải bán" → người bán giỏi nhất một offer chưa chuẩn hoá luôn là founder; sales thuê vào lúc này chết chìm và đốt lương 3 tháng.
2. Thuê rẻ nhất rồi kỳ vọng chất lượng senior → trả theo scorecard thị trường hoặc đừng thuê.
3. Không trial task — phỏng vấn miệng chọn người nói hay, không chọn người làm được.
4. Giao việc không giao chuẩn: "em cứ làm đi" rồi thất vọng → thất vọng đó là lỗi founder.
5. Micro-manage từng thao tác → người giỏi bỏ đi, còn lại người chỉ chờ lệnh — đắt gấp đôi.
6. Uỷ quyền việc nhưng giữ quyết định vặt → mọi thứ vẫn xếp hàng qua founder, thuê như chưa thuê.
7. Giữ người dưới chuẩn quá lâu vì nể/ngại → chuẩn của cả team tụt về mức người thấp nhất được dung túng.
8. Quên rằng khách là của doanh nghiệp: để 1 người ôm toàn bộ quan hệ 1 khách lớn không ai backup → người đi, khách đi theo (biến thể lỗi `04` #11).

## 9. Recovery khi thất bại
- **Người mới dưới chuẩn trong thử việc:** kết thúc đúng hạn thử việc, trả đủ, nói thẳng bằng số đo — kéo dài vì hy vọng là mua thêm thất vọng bằng tiền.
- **Người đang ổn tụt chuẩn 2 tháng:** 1 cuộc nói chuyện thẳng + 1 tháng phục hồi có số đo → không phục hồi: thay. SOP + QC đã có nghĩa là thay người không còn là khủng hoảng.
- **Khách mất niềm tin sau lỗi giao hàng của team:** founder trực tiếp xin lỗi + sửa miễn phí + tự giao 1 chu kỳ cho khách đó; nội bộ: truy về SOP thiếu gì, không truy "ai ngu".
- **Founder nhận ra mình đã thuê quá tay (chi người >45% doanh thu, việc không đủ):** cắt về đúng trigger — đau một lần, chậm nhưng chắc; giữ đội thừa vì sĩ diện là con đường phá sản chậm.
- **Toàn bộ nỗ lực uỷ quyền thất bại lặp lại nhiều người liên tiếp:** vấn đề gần như chắc chắn là SOP hoặc founder (giao chuẩn mơ hồ, đổi ý liên tục) → dừng tuyển, sửa gốc trước, xem `04` Recovery.

## 10. Công cụ AI phù hợp
- AI viết nháp scorecard, SOP đào tạo, trial task từ SOP gốc — founder duyệt.
- AI chấm sơ bộ trial task theo rubric (người quyết cuối).
- Agent tổng hợp số 1-1 hằng tuần từ tracker để 1-1 chỉ bàn việc gỡ kẹt.
- Loom/video ngắn + AI transcript → mỗi lần founder làm mẫu một việc là một tài liệu đào tạo tự sinh ra.

## 11. KPI
- **Input KPI:** % việc giao ra có SOP + scorecard; tỷ lệ kiểm mẫu đúng lịch.
- **Output KPI:** giờ founder trong giao hàng/tuần (giảm theo quý); doanh thu/người.
- **Leading KPI:** QC pass rate của từng người; thời gian người mới đạt chuẩn (phải ngắn dần — đo chất lượng SOP).
- **Lagging KPI:** churn khách trên tập khách do team phục vụ vs do founder phục vụ (chênh lệch phải → 0); chi phí người/doanh thu; % người vượt thử việc.
- **North Star:** **doanh thu vận hành được không cần founder trong ngày thường** — đo bằng bài kiểm tra vắng mặt hằng quý.

## 12. Trigger để Pivot (cách tổ chức)
- 3 người liên tiếp thất bại ở cùng một vai → vai đó thiết kế sai (gộp 2 việc khác bản chất, hoặc nên là agent + người kiểm) → thiết kế lại vai, không tuyển tiếp.
- Chi người/doanh thu >45% hai tháng liền → cấu trúc sai: tăng giá, tự động hoá thêm, hoặc cắt lớp — theo thứ tự đó.

## 13. Trigger để Scale (đội)
- Người hiện tại QC >90% + việc tồn >120% công suất 4 tuần liên tiếp + runway 3 tháng cho người mới → tuyển bản sao của vai đã chạy tốt (nhân vai đã chứng minh, không mở vai mới khi vai cũ chưa ổn).
- ≥8–10 khách active và founder vẫn là đầu mối mọi khách → lên nấc account manager.

## 14. Trigger để Thuê người (đệ quy — thuê người quản người)
- ≥4–5 người giao hàng và founder mất >8h/tuần cho điều phối → nâng người giỏi nhất lên lead (kèm tăng tiền + scorecard mới), không tuyển "manager chuyên nghiệp" từ ngoài vào đội 5 người.

## 15. Trigger để dùng AI Agent (thay vì thuê)
- Mọi việc định thuê phải qua cổng (b) ở §5 trước: agent rẻ hơn, không nghỉ việc, không cần động viên — nhưng chỉ cho việc 4/4 tiêu chí.
- Mỗi quý rà lại việc đang thuê người: việc nào agent nay đã làm được 90%? → chuyển dần, người chuyển lên việc phán đoán cao hơn (nâng cấp vai, không nhất thiết cắt người — người hiểu ngách là tài sản khó thay).

## 16. Trigger để KHÔNG dùng AI
- Tuyển chọn cuối cùng, đánh giá thử việc, quyết định thay người — người quyết, 100%; AI chỉ chuẩn bị dữ liệu.
- 1-1, phản hồi khó, tăng lương, chia tay — trực tiếp, không uỷ cho AI soạn nguyên văn (con người nghe ra giọng máy trong tin xấu, và sẽ không tha thứ).
- Quan hệ với khách trả tiền cao nhất — luôn có tên người cụ thể chịu trách nhiệm, không phải "hệ thống".

## 17. Nguyên tắc dài hạn
- Sản phẩm thật của founder từ G2 trở đi không phải deliverable — là **cỗ máy tạo ra deliverable**: SOP, chuẩn chất lượng, con người đúng chỗ, agent đúng việc. Mỗi giờ founder làm việc "trong" doanh nghiệp là một giờ không xây "cỗ máy" đó.
- Trật tự bất biến: **Do → Document → Delegate → Audit → Own.** Nhảy cóc từ Do sang Delegate (bỏ Document) là nguồn của 80% thất bại uỷ quyền.
- Đội nhỏ nhất hoàn thành việc là đội đúng. Trong kỷ nguyên agent, đội 3–5 người + hạm đội agent có thể vận hành mức doanh thu trước đây cần 15 người — lợi thế cấu trúc lớn nhất của người xây doanh nghiệp sau 2025, đừng tự bỏ nó bằng cách tuyển theo quán tính thời cũ.

**Self-critique:** *Về số 0, tôi có cần skill này không?* — **Chưa — và dùng sớm là có hại** (chi phí cố định giết G0–G1). Nhưng đến G2 nó là bắt buộc: mọi con đường trong `05` đều nghẽn ở cùng một cổ chai — 24 giờ của founder. Tăng giá nới cổ chai được vài lần; sau đó chỉ còn hai đòn bẩy là người và agent, và skill này tồn tại để dùng cả hai đúng trật tự, đúng liều.
