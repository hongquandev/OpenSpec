# Các Lệnh (Commands)

Đây là tài liệu tham khảo cho các lệnh slash commands của OpenSpec. Các lệnh này được gọi trong giao diện chat của trợ lý lập trình AI của bạn (ví dụ: Claude Code, Cursor, Windsurf).

Để biết các mẫu quy trình công việc và khi nào nên sử dụng từng lệnh, hãy xem [Workflows](workflows.md). Đối với các lệnh CLI, hãy xem [CLI](cli.md).

## Tham chiếu nhanh

| Lệnh | Mục đích |
|---------|---------|
| `/opsx:explore` | Suy nghĩ thấu đáo các ý tưởng trước khi cam kết thực hiện một change |
| `/opsx:new` | Bắt đầu một change mới |
| `/opsx:continue` | Tạo artifact tiếp theo dựa trên các dependency |
| `/opsx:ff` | Fast-forward: tạo tất cả các artifacts lập kế hoạch cùng một lúc |
| `/opsx:apply` | Thực hiện các task từ change |
| `/opsx:verify` | Xác minh việc implementation khớp với các artifacts |
| `/opsx:sync` | Merge delta specs vào main specs |
| `/opsx:archive` | Lưu trữ một change đã hoàn thành |
| `/opsx:bulk-archive` | Lưu trữ nhiều changes cùng một lúc |
| `/opsx:onboard` | Hướng dẫn thực hành qua toàn bộ quy trình làm việc |

---

## Tham chiếu Lệnh Chi tiết

### `/opsx:explore`

Suy nghĩ thấu đáo các ý tưởng, điều tra các vấn đề và làm rõ các yêu cầu trước khi cam kết thực hiện một change.

**Cú pháp:**
```
/opsx:explore [topic]
```

**Nó làm gì:**
- Mở một cuộc hội thoại khám phá không yêu cầu cấu trúc cố định
- Điều tra codebase để trả lời các câu hỏi
- So sánh các phương án và cách tiếp cận
- Tạo sơ đồ trực quan để làm rõ tư duy
- Có thể chuyển sang `/opsx:new` khi các ý tưởng đã kết tinh

---

### `/opsx:new`

Bắt đầu một change mới. Tạo cấu trúc thư mục change và chuẩn bị dựa trên schema đã chọn.

**Cú pháp:**
```
/opsx:new [change-name] [--schema <schema-name>]
```

**Nó làm gì:**
- Tạo thư mục `openspec/changes/<change-name>/`
- Tạo tệp metadata `.openspec.yaml` trong thư mục change
- Hiển thị template artifact đầu tiên sẵn sàng để tạo

---

### `/opsx:continue`

Tạo artifact tiếp theo trong chuỗi phụ thuộc (dependency chain). Tạo từng artifact một để tiến triển dần dần.

**Cú pháp:**
```
/opsx:continue [change-name]
```

**Nó làm gì:**
- Truy vấn biểu đồ phụ thuộc artifact
- Hiển thị artifact nào đã sẵn sàng so với những loại đang bị chặn (blocked)
- Tạo artifact sẵn sàng đầu tiên
- Đọc các tệp phụ thuộc để lấy ngữ cảnh

---

### `/opsx:ff`

Fast-forward (Tiến nhanh) qua việc tạo artifact. Tạo tất cả các artifacts lập kế hoạch cùng một lúc.

**Cú pháp:**
```
/opsx:ff [change-name]
```

**Nó làm gì:**
- Tạo tất cả các artifacts theo thứ tự phụ thuộc
- Theo dõi tiến độ qua danh sách việc cần làm (todo list)
- Dừng lại khi tất cả các artifacts cần thiết để `apply` đã hoàn thành

---

### `/opsx:apply`

Thực hiện các task từ change. Làm việc thông qua danh sách task, viết code và đánh dấu các mục đã hoàn thành.

**Cú pháp:**
```
/opsx:apply [change-name]
```

**Nó làm gì:**
- Đọc `tasks.md` và xác định các task chưa hoàn thành
- Thực hiện các task từng bước một
- Viết code, tạo tệp, chạy test khi cần thiết
- Đánh dấu các task hoàn thành bằng dấu `[x]`

---

### `/opsx:verify`

Xác minh rằng việc thực hiện (implementation) khớp với các artifacts của change. Kiểm tra tính đầy đủ, chính xác và mạch lạc.

**Cú pháp:**
```
/opsx:verify [change-name]
```

**Nó làm gì:**
- Kiểm tra ba khía cạnh chất lượng thực hiện:
    *   **Tính đầy đủ (Completeness)**: Tất cả task đã xong, yêu cầu đã được thực hiện.
    *   **Tính chính xác (Correctness)**: Implementation khớp với ý định của spec, các trường hợp biên được xử lý.
    *   **Tính mạch lạc (Coherence)**: Quyết định thiết kế được phản ánh trong code, các mẫu (patterns) nhất quán.

---

### `/opsx:sync`

**Lệnh tùy chọn.** Merge các delta specs từ một change vào main specs. Lệnh Archive sẽ nhắc bạn sync nếu cần, vì vậy bạn thường không cần chạy lệnh này thủ công.

---

### `/opsx:archive`

Lưu trữ một change đã hoàn thành. Hoàn tất change và di chuyển nó vào thư mục archive.

**Cú pháp:**
```
/opsx:archive [change-name]
```

---

### `/opsx:bulk-archive`

Lưu trữ nhiều changes đã hoàn thành cùng một lúc. Xử lý các xung đột spec giữa các changes.

---

### `/opsx:onboard`

Hướng dẫn thực hành qua toàn bộ quy trình làm việc của OpenSpec. Một hướng dẫn tương tác sử dụng chính codebase thực tế của bạn.

**Cú pháp:**
```
/opsx:onboard
```

---

## Cú pháp lệnh theo Công cụ AI

Các công cụ AI khác nhau sử dụng cú pháp lệnh hơi khác nhau:

| Công cụ | Ví dụ Cú pháp |
|------|----------------|
| Claude Code | `/opsx:new`, `/opsx:apply` |
| Cursor | `/opsx-new`, `/opsx-apply` |
| Windsurf | `/opsx-new`, `/opsx-apply` |
| Copilot | `/opsx-new`, `/opsx-apply` |

---

## Các lệnh cũ (Legacy Commands)

Các lệnh này sử dụng quy trình làm việc "tất cả trong một" cũ hơn. Chúng vẫn hoạt động nhưng các lệnh OPSX được khuyến nghị hơn.

| Lệnh | Nó làm gì |
|---------|--------------|
| `/openspec:proposal` | Tạo tất cả các artifacts cùng lúc |
| `/openspec:apply` | Thực hiện change |
| `/openspec:archive` | Lưu trữ change |

---

## Giải quyết sự cố

### "Change not found"
Lệnh không thể xác định change nào cần làm việc.
**Giải pháp**: Chỉ định tên change rõ ràng: `/opsx:apply add-dark-mode`.

### "No artifacts ready"
Tất cả các artifacts đã hoàn thành hoặc bị chặn bởi các phụ thuộc còn thiếu.
**Giải pháp**: Chạy `openspec status --change <name>` để xem điều gì đang gây chặn.

---

## Các bước tiếp theo

- [Workflows](workflows.md) - Các mẫu phổ biến và khi nào sử dụng từng lệnh
- [CLI](cli.md) - Các lệnh terminal để quản lý và xác minh
- [Customization](customization.md) - Tạo các schemas và workflows tùy chỉnh
- [Concepts](concepts.md) - Hiểu sâu hơn về specs, changes và schemas
