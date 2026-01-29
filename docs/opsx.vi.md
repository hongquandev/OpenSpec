# Quy trình OPSX (Workflow)

> **Khả năng tương thích:** Hiện tại chỉ dành cho Claude Code. Hoan nghênh mọi phản hồi trên [Discord](https://discord.gg/YctCnvvshC).

## Nó là gì?

OPSX là một **quy trình làm việc linh hoạt, lặp lại** cho các thay đổi trong OpenSpec. Không còn các giai đoạn cứng nhắc — chỉ là các hành động bạn có thể thực hiện bất cứ lúc nào.

## Tại sao nó tồn tại?

Quy trình OpenSpec tiêu chuẩn hoạt động tốt, nhưng nó bị **khóa chặt**:

- **Hướng dẫn được hardcode** — bị chôn vùi trong mã nguồn, bạn không thể thay đổi chúng.
- **Tất cả hoặc không có gì** — một lệnh lớn tạo ra mọi thứ, không thể kiểm tra từng phần nhỏ.
- **Cấu trúc cố định** — cùng một quy trình cho tất cả mọi người, không có sự tùy biến.
- **Hộp đen** — khi kết quả AI không tốt, bạn không thể tinh chỉnh các prompts.

**OPSX mở ra tất cả.** Bây giờ bất kỳ ai cũng có thể:

1. **Thử nghiệm với các hướng dẫn** — chỉnh sửa một template, xem AI có làm tốt hơn không.
2. **Kiểm tra chi tiết** — xác minh hướng dẫn của từng artifact một cách độc lập.
3. **Tùy chỉnh quy trình** — định nghĩa các artifacts và các phụ thuộc (dependencies) của riêng bạn.
4. **Lặp lại nhanh chóng** — thay đổi một template, kiểm tra ngay lập tức.

## Trải nghiệm Người dùng

**Vấn đề với quy trình tuyến tính:**
Bạn đang "ở giai đoạn lập kế hoạch", sau đó là "giai đoạn thực hiện", rồi "hoàn thành". Nhưng thực tế công việc không diễn ra như vậy. Bạn thực hiện một phần, nhận ra thiết kế sai, cần cập nhật specs, rồi mới tiếp tục thực hiện.

**Tiếp cận của OPSX:**
- **Hành động, không phải giai đoạn** — tạo, thực hiện, cập nhật, lưu trữ — thực hiện bất kỳ điều gì vào bất cứ lúc nào.
- **Phụ thuộc là yếu tố thúc đẩy** — chúng cho thấy điều gì là khả thi tiếp theo, không phải điều gì bắt buộc phải theo trình tự.

---

## Thiết lập (Setup)

```bash
# 1. Đảm bảo bạn đã cài đặt và khởi tạo openspec
openspec init

# 2. Tạo các experimental skills
openspec experimental
```

Trong quá trình thiết lập, bạn sẽ được nhắc tạo **cấu hình dự án** (`openspec/config.yaml`).

---

## Các lệnh (Commands)

| Lệnh | Nó làm gì |
|---------|--------------|
| `/opsx:explore` | Suy nghĩ các ý tưởng, điều tra vấn đề, làm rõ yêu cầu |
| `/opsx:new` | Bắt đầu một change mới |
| `/opsx:continue` | Tạo artifact tiếp theo (dựa trên những gì đã sẵn sàng) |
| `/opsx:ff` | Fast-forward — tạo tất cả các artifacts lập kế hoạch cùng lúc |
| `/opsx:apply` | Thực hiện các task (implementation) |
| `/opsx:sync` | Đồng bộ delta specs vào bản chính |
| `/opsx:archive` | Lưu trữ khi hoàn thành |

---

## Cách sử dụng

### Khám phá một ý tưởng
```
/opsx:explore
```
Suy nghĩ thấu đáo các ý tưởng, điều tra vấn đề, so sánh các phương án. Không yêu cầu cấu trúc — chỉ như một cộng sự cùng suy nghĩ.

### Bắt đầu một change mới
```
/opsx:new
```
Bạn sẽ được hỏi muốn xây dựng điều gì và sử dụng schema workflow nào.

### Tạo artifacts
```
/opsx:continue
```
Hiển thị những gì sẵn sàng để tạo dựa trên các phụ thuộc, sau đó tạo một artifact.

```
/opsx:ff add-dark-mode
```
Tạo tất cả các planning artifacts cùng lúc.

### Thực hiện (phần linh hoạt)
```
/opsx:apply
```
Làm việc thông qua các task, đánh dấu hoàn thành khi thực hiện xong. Nếu bạn đang xử lý nhiều changes, hãy dùng `/opsx:apply <name>`.

---

## Khi nào nên Cập nhật vs. Bắt đầu mới

Bạn luôn có thể chỉnh sửa proposal hoặc specs trước khi thực hiện. Nhưng khi nào việc tinh chỉnh trở thành "một công việc khác hoàn toàn"?

### Cập nhật Change hiện có khi:
- **Cùng ý định, tinh chỉnh cách thực hiện**: Phát hiện các trường hợp biên, tinh chỉnh cách tiếp cận nhưng mục tiêu không đổi.
- **Phạm vi (Scope) thu hẹp**: Nhận ra phạm vi đầy đủ quá lớn, muốn ra mắt MVP trước.
- **Điều chỉnh dựa trên thực tế**: Codebase không có cấu trúc như bạn nghĩ, một dependency không hoạt động như mong đợi.

### Bắt đầu một Change mới khi:
- **Ý định thay đổi cơ bản**: Bản chất vấn đề hiện tại đã khác. (Ví dụ: "Thêm dark mode" chuyển thành "Xây dựng hệ thống theme toàn diện").
- **Phạm vi bùng nổ**: Công việc phát sinh quá nhiều đến mức nó thành một việc khác.
- **Bản gốc có thể hoàn thành**: Công việc cũ có thể đánh dấu "xong", công việc mới đứng độc lập.

---

## Kiến trúc: Phụ thuộc (Dependency Graph)

Các artifacts tạo thành một biểu đồ có hướng. Phụ thuộc giúp kích hoạt các bước tiếp theo:

```
    proposal ──► specs ──► design ──► tasks ──► apply
```

Lệnh `/opsx:continue` kiểm tra những gì sẵn sàng (READY) và đề xuất artifact tiếp theo. Một trạng thái được coi là READY khi tất cả các phụ thuộc của nó đã xong (DONE - tệp tồn tại trên hệ thống).

---

## Lời khuyên

- Sử dụng `/opsx:explore` trước khi cam kết thực hiện một change.
- Sử dụng `/opsx:ff` khi bạn đã biết rõ mình muốn gì, và `/opsx:continue` khi đang tìm tòi.
- Trong lúc `/opsx:apply`, nếu thấy sai — hãy sửa artifact, rồi tiếp tục.
- Theo dõi tiến độ qua các checkbox trong `tasks.md`.
- Kiểm tra trạng thái bất cứ lúc nào: `openspec status --change "tên_change"`.
