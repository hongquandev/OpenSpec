# Quy trình làm việc (Workflows)

Hướng dẫn này trình bày các mẫu quy trình làm việc phổ biến cho OpenSpec và khi nào nên sử dụng chúng. Để thiết lập cơ bản, hãy xem [Bắt đầu](getting-started.md). Để tham chiếu lệnh, hãy xem [Commands](commands.md).

## Triết lý: Hành động, Không phải Giai đoạn

Các quy trình truyền thống bắt bạn đi qua các giai đoạn: lập kế hoạch, thực hiện, rồi hoàn thành. Nhưng thực tế công việc không diễn ra ngăn nắp như vậy.

OPSX chọn một cách tiếp cận khác:
- **Hành động, không phải giai đoạn** - Các lệnh là những thứ bạn có thể làm, không phải các giai đoạn mà bạn bị mắc kẹt trong đó.
- **Phụ thuộc là yếu tố thúc đẩy** - Chúng cho thấy điều gì là khả thi tiếp theo, không phải điều gì bắt buộc phải thực hiện.

## Các mẫu Quy trình làm việc

### Tính năng Nhanh (Quick Feature)
Khi bạn đã biết mình muốn xây dựng điều gì và chỉ cần thực hiện:

```text
/opsx:new ──► /opsx:ff ──► /opsx:apply ──► /opsx:verify ──► /opsx:archive
```
Phù hợp cho: Các tính năng nhỏ đến trung bình, fix bug, các thay đổi rõ ràng.

### Khám phá (Exploratory)
Khi yêu cầu chưa rõ ràng hoặc bạn cần điều tra trước:

```text
/opsx:explore ──► /opsx:new ──► /opsx:continue ──► ... ──► /opsx:apply
```
Phù hợp cho: Tối ưu hóa hiệu năng, gỡ lỗi (debugging), quyết định kiến trúc, yêu cầu chưa rõ ràng.

### Các thay đổi Song song (Parallel Changes)
Làm việc trên nhiều thay đổi cùng một lúc:

```text
Change A: /opsx:new ──► /opsx:ff ──► /opsx:apply (đang thực hiện)
                                         │
                                   chuyển ngữ cảnh
                                         │
Change B: /opsx:new ──► /opsx:ff ──────► /opsx:apply
```
Khi bạn có nhiều changes đã hoàn thành, hãy sử dụng `/opsx:bulk-archive` để lưu trữ tất cả cùng lúc.

---

## Hoàn tất một Change

Quy trình hoàn tất được khuyến nghị:
```text
/opsx:apply ──► /opsx:verify ──► /opsx:archive
```

#### Verify: Kiểm tra công việc của bạn
`/opsx:verify` xác minh việc thực hiện so với các artifacts của bạn trên ba khía cạnh:
- **Tính đầy đủ (Completeness)**: Tất cả task đã xong, mọi yêu cầu đều có code tương ứng.
- **Tính chính xác (Correctness)**: Implementation khớp với ý định của spec, xử lý các trường hợp biên.
- **Tính mạch lạc (Coherence)**: Quyết định thiết kế được phản ánh trong cấu trúc code, quy tắc đặt tên nhất quán.

#### Archive: Hoàn tất Change
`/opsx:archive` hoàn tất change và di chuyển nó vào thư mục archive. Nếu các specs chưa được đồng bộ (sync), lệnh này sẽ nhắc bạn thực hiện.

---

## Khi nào nên sử dụng lệnh nào?

### `/opsx:ff` vs `/opsx:continue`
- Sử dụng `/opsx:ff`: Khi yêu cầu rõ ràng, sẵn sàng để xây dựng, cần di chuyển nhanh.
- Sử dụng `/opsx:continue`: Khi đang khám phá, muốn xem xét từng bước, muốn thay đổi proposal trước khi tạo specs.

### Khi nào nên Cập nhật vs Bắt đầu mới
- **Cập nhật change hiện có khi**: Cùng ý định nhưng tinh chỉnh cách thực hiện, thu hẹp phạm vi, điều chỉnh dựa trên những gì học được khi code.
- **Bắt đầu change mới khi**: Ý định thay đổi cơ bản, phạm vi bùng nổ thành một công việc hoàn toàn khác, công việc cũ có thể đứng độc lập.

---

## Các thực hành tốt nhất (Best Practices)

- **Giữ các Changes tập trung**: Mỗi change nên là một đơn vị công việc logic duy nhất. Tránh gộp "thêm tính năng X và refactor Y" vào cùng một change.
- **Sử dụng `/opsx:explore` cho các yêu cầu chưa rõ ràng**: Khám phá không gian vấn đề trước khi cam kết tạo artifacts.
- **Xác minh (Verify) trước khi lưu trữ**: Sử dụng `/opsx:verify` để bắt các lỗi sai lệch trước khi đóng change.
- **Đặt tên Change rõ ràng**: Sử dụng tên mô tả như `add-dark-mode` hay `fix-login-redirect` thay vì `feature-1` hay `update`.
