# 00 — Hiến pháp vận hành (Operating Constitution)

Mọi skill, SOP và quyết định trong OS này phải tuân theo file này. Khi hai tài liệu mâu thuẫn, file này thắng.

## 1. Thứ tự ưu tiên khi ra quyết định

Khi so sánh hai lựa chọn bất kỳ, chấm theo thứ tự — tiêu chí trên thắng tiêu chí dưới:

1. **Xác suất có khách hàng** — có ai đang chủ động tìm và trả tiền cho việc này không?
2. **Xác suất tạo doanh thu** — từ tiếp cận đến tiền về mất bao nhiêu bước?
3. **ROI** — lợi nhuận / (tiền + giờ bỏ ra)
4. **Tốc độ triển khai** — tính bằng ngày, không bằng tháng
5. **Khả năng mở rộng**
6. **Khả năng tự động hóa**
7. **Độ bền dài hạn**

Hệ quả quan trọng: một mô hình scale kém nhưng ra tiền trong 2 tuần **thắng** một mô hình scale vô hạn nhưng cần 6 tháng mới có khách đầu tiên — *khi bạn đang ở số 0*. Thứ tự ưu tiên đảo dần khi vốn tăng (xem `05-roadmap-tong.md`).

## 2. Quy tắc ra quyết định toàn cục

```
IF chưa có khách hàng trả tiền nào
THEN mọi giờ làm việc chia 2 loại: (a) nói chuyện với khách tiềm năng, (b) giao hàng cho khách đã trả tiền
ELSE-IF đang làm việc không thuộc (a) hoặc (b)
THEN dừng lại — đó gần như chắc chắn là trì hoãn được nguỵ trang (làm logo, làm website, học thêm tool)

IF phân vân giữa xây sản phẩm và bán
THEN bán trước: thu tiền hoặc cam kết bằng văn bản TRƯỚC khi xây
     (bán thứ chưa xây = mô tả kết quả + thời hạn giao; nếu không ai mua, bạn vừa tiết kiệm 3 tháng)

IF phân vân giữa công nghệ mới và công nghệ đang ra tiền
THEN chọn thứ đang ra tiền; công nghệ mới chỉ đáng dùng khi nó giảm chi phí giao hàng ≥30% cho khách HIỆN TẠI

IF một việc lặp lại ≥3 lần/tuần và có quy trình rõ
THEN viết SOP → thử giao AI → nếu AI đạt ≥90% chất lượng thì tự động hoá
ELSE giữ con người làm (đặc biệt: bán hàng, đàm phán, quan hệ khách, quyết định chiến lược)
```

## 3. Khi nào dùng AI, khi nào dùng người

| Việc | Giao cho |
|---|---|
| Nghiên cứu thị trường, tóm tắt, phân loại | AI — gần như hoàn toàn |
| Bản nháp đầu (content, code, email, proposal) | AI nháp → người duyệt |
| Cá nhân hoá outreach, trả lời khách, chốt deal | Người (AI chỉ chuẩn bị tư liệu) |
| Giao hàng lặp lại theo SOP đã chuẩn | AI Agent, người kiểm mẫu ngẫu nhiên 10–20% |
| Quyết định giá, pivot, thuê người | Người — luôn luôn |

**Trigger KHÔNG dùng AI:** khi lỗi của AI làm mất khách hoặc mất uy tín mà không có bước người-kiểm-tra ở giữa; khi việc cần chịu trách nhiệm pháp lý; khi khối lượng quá nhỏ để đáng công viết prompt/quy trình.

## 4. Quy tắc tiền mặt (Cash Rules)

1. **Doanh thu trước, hạ tầng sau.** Không mua tool trả phí khi bản miễn phí còn dùng được. Ngân sách tool tháng đầu: 0–20 USD.
2. **Lấy cọc 30–50%** trước khi bắt đầu bất kỳ dự án nào. Không cọc = không phải khách.
3. **Không đầu tư trước doanh thu** vào quảng cáo trả phí khi chưa có offer đã được kiểm chứng bằng bán tay (manual sales).
4. Tách riêng tiền doanh nghiệp — từ đồng đầu tiên.

## 5. Nhịp vận hành chuẩn (mọi giai đoạn)

- **Hằng ngày:** 1 khối 2–4 giờ cho việc tạo doanh thu trực tiếp (outreach/bán/giao hàng) TRƯỚC mọi việc khác.
- **Hằng tuần (30 phút, cố định lịch):** xem KPI → so với trigger Pivot/Scale trong skill tương ứng → quyết định 1 thay đổi duy nhất cho tuần sau. Không đổi nhiều hơn 1 biến/tuần, nếu không sẽ không biết cái gì có tác dụng.
- **Hằng tháng:** đối chiếu với Exit Criteria của giai đoạn hiện tại trong `05-roadmap-tong.md`.

## 6. Định nghĩa dùng chung (để các chapter không phải lặp lại)

- **ICP** (Ideal Customer Profile): chân dung khách lý tưởng — ngành, quy mô, vai trò người quyết định, nỗi đau, ngân sách.
- **Offer:** lời chào hàng = kết quả cụ thể + thời hạn + giá + cam kết rủi ro (ví dụ: "30 bài SEO chuẩn chỉnh trong 30 ngày, không đạt X thì hoàn tiền").
- **Productized service:** dịch vụ đóng gói — phạm vi cố định, giá cố định, quy trình cố định; đối lập với "làm theo giờ".
- **Retainer:** khách trả cố định hằng tháng cho một phạm vi công việc lặp lại.
- **Qualified conversation:** cuộc trao đổi 2 chiều với đúng ICP về đúng nỗi đau — đơn vị đo quan trọng nhất giai đoạn đầu.
