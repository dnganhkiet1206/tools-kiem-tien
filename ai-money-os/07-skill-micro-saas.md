# 07 — SKILL: Micro SaaS trích xuất từ dịch vụ

Đường thoát khỏi bán giờ — nhưng chỉ mở khi dịch vụ (`02`) đã chỉ ra chính xác phải xây gì và bán cho ai. Đây là phiên bản Micro SaaS **duy nhất** sống sót qua sàng lọc `01`; mọi biến thể "nghĩ ra ý tưởng app rồi đi tìm người dùng" đã bị loại ở đó.

## 1. Tên Skill
Trích xuất một nỗi đau lặp lại từ tập khách dịch vụ thành sản phẩm phần mềm ngách, presale trước khi viết dòng code đầu tiên, xây ≤4 tuần bằng AI coding, phân phối qua kênh và uy tín đã có.

## 2. Mục tiêu
Thêm một lớp doanh thu MRR có margin ~90% và không tỷ lệ thuận với giờ lao động; dài hạn: đổi cấu trúc doanh nghiệp từ "dịch vụ + tool" sang "sản phẩm + dịch vụ cao cấp".

## 3. Khi nào dùng — điều kiện cứng, đủ CẢ BA
1. **≥10 khách/khách tiềm năng cùng ngách than cùng một nỗi đau** — đếm được, có ghi chép, không phải cảm giác.
2. **Bạn đã giải nỗi đau đó thủ công/bán thủ công ≥10 lần** — SOP giao hàng của `02` chính là spec sản phẩm; nếu chưa có SOP, sản phẩm sẽ là phỏng đoán.
3. **Có ≥1 kênh phân phối sẵn:** tập khách dịch vụ + danh sách email + vị thế ngách từ `06`.

## 4. Khi nào KHÔNG dùng
- Thiếu bất kỳ điều kiện nào ở §3 → quay lại làm dịch vụ, chưa đến lúc.
- Đang ở G0–G1 (`05`) → SaaS lúc này là lỗi `04` #2 (xây trước bán sau) mặc áo đẹp.
- Nỗi đau đã có sẵn tool tốt–rẻ ngoài thị trường → bán tư vấn triển khai tool đó (dịch vụ) thay vì xây bản nhái; chỉ xây khi tool hiện có *thật sự* hỏng với ngách của bạn ở điểm cụ thể nói tên được.
- Động cơ thật là "chán dịch vụ, muốn làm product cho oai" → động cơ sai, sản phẩm sẽ chết và dịch vụ cũng hỏng theo.

## 5. Framework ra quyết định

```
IF ≥10 khách cùng nỗi đau AND đã giải thủ công ≥10 lần AND có kênh phân phối
THEN chạy PRESALE (§6) — chưa viết code
ELSE quay lại `02`, tiếp tục gom dữ liệu

IF presale: ≥5 khách TRẢ TIỀN TRƯỚC (không phải "hay đấy, làm đi anh ủng hộ")
     trong ≤3 tuần chào hàng
THEN xây MVP 4 tuần
ELSE-IF 2–4 người trả
THEN phỏng vấn từng người từ chối → nếu lý do là giá/thiếu 1 tính năng cụ thể → sửa offer, chào lại 1 vòng
ELSE (<2) → hoàn tiền, huỷ dự án, ghi vào khám nghiệm `04` — chi phí thất bại: 3 tuần, 0 code, 0 đồng.
     Đây là THÀNH CÔNG của quy trình, không phải thất bại: nó vừa cứu bạn 6 tháng.

IF trong lúc xây, muốn thêm tính năng ngoài SOP gốc
THEN không. MVP = đúng workflow SOP đã bán, không hơn.
```

## 6. SOP từng bước

```
INPUT        SOP giao hàng đã chạy ≥10 lần + danh sách ≥10 khách cùng nỗi đau + kênh phân phối
   ↓
RESEARCH     (tuần 0) Phỏng vấn 5 khách: họ đang giải bằng gì, mất bao nhiêu tiền/giờ mỗi tháng,
             họ mô tả nỗi đau bằng từ ngữ nào (từ ngữ này = copy bán hàng) · rà tool hiện có: hỏng đâu
   ↓
DECISION     Offer sản phẩm 4-phần (kết quả + thời hạn có bản dùng + giá + đảm bảo hoàn tiền) ·
             giá neo theo dịch vụ: ~1/4 đến 1/5 retainer tháng (mua tool rẻ hơn thuê bạn 5 lần —
             lời chào tự bán) · chốt ranh giới: dịch vụ giữ phần nào, tool thay phần nào
   ↓
EXECUTION    PRESALE 3 tuần theo §5 → đạt ngưỡng → XÂY 4 tuần bằng AI coding:
             tuần 1–2 lõi workflow · tuần 3 khách presale dùng bản thô, sửa theo phản hồi ·
             tuần 4 thanh toán + onboarding tự phục vụ · stack nhàm chán, không sáng tạo hạ tầng
   ↓
MEASUREMENT  Activation (% khách đạt kết quả đầu trong 24h) · usage tuần · MRR · churn tháng ·
             số phút support/khách
   ↓
OPTIMIZATION Tháng: gọi mọi khách churn (lý do thật) · sửa MỘT điểm gãy activation lớn nhất ·
             tăng giá cho cohort mới khi activation >60% và churn <5%
   ↓
AUTOMATION   Onboarding, hướng dẫn, billing, email vòng đời: tự động từ ngày 1 (AI dựng nhanh) ·
             support: AI trả lời tầng 1 từ tài liệu, người xử tầng 2
   ↓
SCALE        Phân phối lặp được: case study khách presale lên kênh `06` → khách dịch vụ mới
             mặc định kèm tool → affiliate cho người có audience ngách (trả 20–30% recurring —
             affiliate đứng ĐÚNG chỗ của nó trong OS: kênh của người khác bán sản phẩm của bạn)
```

### 6b. SOP theo nhịp
**Daily (≤1h giai đoạn đầu):** support tầng 2 · xem số activation của khách mới hôm qua.
**Weekly:** ship 1 cải tiến nhỏ từ phản hồi thật (không phải từ tưởng tượng) · cập nhật 5 số đo · 1 case study/mẩu content sản phẩm đẩy sang `06`.
**Monthly:** gọi khách churn · quyết định giá/packaging · đối chiếu ranh giới dịch vụ–sản phẩm (có đang tự ăn thịt retainer không? xem §8.5).
**Quarterly:** sát hạch: MRR product vs giờ bỏ vào — nếu 2 quý liền MRR/giờ thấp hơn dịch vụ/giờ → §12.

## 7. Checklist trước khi viết dòng code đầu tiên
- [ ] Đủ 3 điều kiện cứng §3, có số liệu ghi chép
- [ ] ≥5 khách trả tiền trước, tiền đã nằm trong tài khoản
- [ ] Spec = SOP giao hàng hiện có, phạm vi khoá cứng 4 tuần
- [ ] Giá + đảm bảo hoàn tiền công bố rõ với khách presale
- [ ] Stack chọn xong theo tiêu chí nhàm chán, AI coding dựng được nhanh
- [ ] Ranh giới sản phẩm–dịch vụ viết thành 1 đoạn (cái gì tool làm, cái gì vẫn là retainer)

## 8. Sai lầm phổ biến
1. Bỏ presale vì "chắc chắn họ sẽ mua" → 6 tháng xây cho 0 khách — lỗi giết nhiều builder nhất.
2. Xây cho nỗi đau mình *thấy thú vị* thay vì nỗi đau khách *đã trả tiền* để giải.
3. Phạm vi phình trong 4 tuần xây ("thêm cái dashboard nữa thôi") → 4 tuần thành 4 tháng.
4. Định giá theo chi phí ("AI xây rẻ mà, lấy 9$ thôi") thay vì theo giá trị thay thế → thu không đủ nuôi support.
5. Tool tự ăn thịt dịch vụ: bán tool 200$/tháng làm khách huỷ retainer 1.000$/tháng → thiết kế ranh giới từ DECISION: tool giải 1 lát, dịch vụ giữ lát chiến lược + tuỳ biến.
6. Sáng tạo hạ tầng (microservices, stack mới ra mắt) cho 20 người dùng → nợ kỹ thuật + nợ thời gian.
7. Coi thường support: mỗi khách SMB = câu hỏi lặp → không có tài liệu + AI tầng 1 thì founder thành call center.
8. Ngừng bán dịch vụ ngay khi có MRR đầu tiên → mất dòng tiền nuôi sản phẩm giai đoạn mong manh nhất.

## 9. Recovery khi thất bại
- **Presale trượt:** đã xử lý trong §5 — hoàn tiền, huỷ, ghi khám nghiệm. Không recovery gì thêm; đừng "xây thử xem" — đó là cách biến thất bại 3 tuần thành thất bại 6 tháng.
- **Presale đậu, activation <40%:** sản phẩm chưa giao được kết quả SOP từng giao tay → ngồi cạnh (call/screen-share) 5 khách xem họ kẹt ở bước nào; sửa đúng bước đó; lặp đến activation >60% TRƯỚC khi nhận thêm khách.
- **Activation tốt, churn >8%/tháng:** nỗi đau có thật nhưng theo mùa/one-off → đổi packaging: gói năm, hoặc chấp nhận định vị one-time-tool giá cao thay vì subscription.
- **MRR đứng im 2 quý:** kênh phân phối bão hoà trong tập hiện có → quay lại `06` mở rộng vị thế ngách, hoặc §12.

## 10. Công cụ AI phù hợp
- **Xây:** AI coding agent (Claude Code hoặc tương đương) — người quyết định kiến trúc + review; stack nhàm chán để AI ít sai.
- **Vận hành:** AI support tầng 1 từ tài liệu; AI viết email vòng đời; AI phân tích ghi chú churn-call theo tháng.
- **Cấm:** để AI tự deploy/migrate dữ liệu khách production không người duyệt — một lần mất dữ liệu khách là chết cả sản phẩm lẫn uy tín dịch vụ (rủi ro lây chéo — xem §17).

## 11. KPI
- **Input KPI:** cải tiến ship/tuần từ phản hồi thật; phút support/khách (phải giảm).
- **Output KPI:** MRR sản phẩm; số khách trả phí.
- **Leading KPI:** activation 24h (>60%); usage tuần (khách dùng hằng tuần hiếm khi churn).
- **Lagging KPI:** churn tháng (<5%); LTV; % MRR từ kênh không phải khách dịch vụ (đo khả năng đứng độc lập).
- **North Star:** **MRR sản phẩm với churn <5%** — MRR churn cao là doanh thu thuê, không phải tài sản.

## 12. Trigger để Pivot
- 2 quý MRR/giờ-bỏ-vào thua dịch vụ/giờ, đã recovery → đóng gói sản phẩm thành "tool nội bộ độc quyền của dịch vụ" (lợi thế cạnh tranh cho `02`, ngừng bán rời) — đây là pivot *lên*, không phải thất bại.
- Nền tảng/model AI mới giải nỗi đau này miễn phí → bán phần workflow + dữ liệu ngách còn lại, không đấu tay bo với nền tảng.

## 13. Trigger để Scale
- Churn <5% + activation >60% + ≥30% MRR từ ngoài tập khách dịch vụ → tăng chi cho phân phối: affiliate recurring, content sản phẩm riêng, thử ads ngách nhỏ (lần đầu tiên trong OS ads được phép — vì đã có LTV đo được để tính CAC trần).
- Khách tự giới thiệu khách ≥2/tháng → thêm chương trình referral trong sản phẩm.

## 14. Trigger để Thuê người
- Support tầng 2 >5h/tuần sau khi AI tầng 1 đã chạy → thuê part-time support (SOP từ chính các câu hỏi lặp).
- Ship chậm hơn danh sách phản hồi thật 2 tháng liền → thuê dev part-time; founder giữ quyền quyết định sản phẩm, không thuê "người nghĩ hộ".

## 15. Trigger để dùng AI Agent
- Support tầng 1, email vòng đời, phân loại phản hồi, monitor lỗi + tạo ticket → agent từ tháng đầu.
- Phân tích usage tuần → agent tổng hợp báo cáo cho weekly review.

## 16. Trigger để KHÔNG dùng AI
- Quyết định roadmap, giá, ranh giới dịch vụ–sản phẩm — người.
- Churn-call — người gọi; kho vàng insight nằm ở đây, AI chỉ được phân tích ghi chú sau.
- Bất kỳ thao tác nào chạm dữ liệu production của khách — người duyệt từng lần.

## 17. Nguyên tắc dài hạn
- Micro SaaS trong OS này không phải "startup" — nó là **bánh đà thứ hai** gắn vào bánh đà dịch vụ: dịch vụ nuôi sản phẩm (tiền, khách, insight), sản phẩm nâng dịch vụ (margin, moat, định vị). Tách rời sớm là gãy cả hai.
- Rủi ro lây chéo phải quản trị chủ động: sự cố sản phẩm làm khách retainer mất niềm tin → chuẩn chất lượng của sản phẩm phải theo chuẩn dịch vụ, không theo chuẩn "MVP startup".
- Đích 3–5 năm (nếu mọi trigger scale đều nổ): sản phẩm ≥50% doanh thu, dịch vụ co lại thành gói triển khai cao cấp — cấu trúc của G3 đường B trong `05`.

**Self-critique:** *Về số 0, tôi có chọn Micro SaaS không?* — **Không — và chương này tồn tại chính vì câu trả lời đó.** Từ số 0, nó đã bị loại ở `01`. Nhưng từ G2 với đủ 3 điều kiện §3, nó là bước đi có kỳ vọng dương rõ nhất: presale đã khử rủi ro lớn nhất (không ai mua), SOP đã khử rủi ro nhì (xây sai), kênh sẵn đã khử rủi ro ba (không ai biết). Cái chết phổ biến của SaaS là chết trước khi gặp khách — quy trình này đảo ngược thứ tự: gặp khách, cầm tiền, rồi mới xây.
