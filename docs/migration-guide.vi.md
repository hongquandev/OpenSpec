# Di chuyển sang OPSX (Migration Guide)

Hướng dẫn này giúp bạn chuyển đổi từ quy trình OpenSpec cũ sang OPSX. Quá trình di chuyển được thiết kế để diễn ra suôn sẻ—mọi công việc hiện tại của bạn sẽ được bảo toàn và hệ thống mới sẽ linh hoạt hơn.

## Có gì thay đổi?

OPSX thay thế quy trình làm việc theo các giai đoạn (phase) cố định cũ bằng một cách tiếp cận dựa trên hành động và linh hoạt hơn.

| Khía cạnh | Hệ thống cũ | OPSX |
|--------|--------|------|
| **Lệnh (Commands)** | `/openspec:proposal`, `/openspec:apply`, `/openspec:archive` | `/opsx:new`, `/opsx:continue`, `/opsx:apply`, và nhiều hơn nữa |
| **Workflow** | Tạo tất cả các artifacts cùng một lúc | Tạo từng bước hoặc tất cả cùng lúc—tùy chọn của bạn |
| **Quay lại bước trước** | Khó khăn vì các cổng giai đoạn (phase gates) | Tự nhiên—cập nhật bất kỳ artifact nào vào bất cứ lúc nào |
| **Tùy chỉnh** | Cấu trúc cố định | Dựa trên Schema, hoàn toàn có thể tùy biến |
| **Cấu hình** | `CLAUDE.md` + `project.md` | Cấu hình gọn gàng trong `openspec/config.yaml` |

---

## Trước khi bạn bắt đầu

### Công việc hiện tại của bạn an toàn

Quá trình di chuyển được thiết kế tập trung vào sự bảo toàn:
- **Các changes đang hoạt động** — Được bảo toàn hoàn toàn. Bạn có thể tiếp tục chúng bằng các lệnh OPSX.
- **Các changes đã lưu trữ (Archive)** — Không bị ảnh hưởng. Lịch sử của bạn vẫn còn nguyên vẹn.
- **Specs chính (`openspec/specs/`)** — Không bị ảnh hưởng. Đây là nguồn sự thật (source of truth) của bạn.
- **Nội dung của bạn trong CLAUDE.md, AGENTS.md, v.v.** — Được bảo toàn. Chỉ các khối đánh dấu của OpenSpec bị xóa; mọi thứ bạn viết vẫn giữ nguyên.

### Những gì cần sự chú ý của bạn

Một tệp yêu cầu di chuyển thủ công: **`openspec/project.md`**. Tệp này không bị xóa tự động vì nó có thể chứa ngữ cảnh dự án mà bạn đã viết. Bạn cần:
1. Xem lại nội dung của nó.
2. Di chuyển ngữ cảnh hữu ích sang `openspec/config.yaml`.
3. Xóa tệp khi đã sẵn sàng.

---

## Chạy quá trình di chuyển

Cả lệnh `openspec init` và `openspec update` đều phát hiện các tệp cũ và hướng dẫn bạn quá trình dọn dẹp.

### Sử dụng `openspec init`

Chạy lệnh này nếu bạn muốn thêm các công cụ mới hoặc cấu hình lại các công cụ đã thiết lập:
```bash
openspec init
```

### Sử dụng `openspec update`

Chạy lệnh này nếu bạn chỉ muốn di chuyển và làm mới các công cụ hiện có lên phiên bản mới nhất:
```bash
openspec update
```

---

## Di chuyển project.md sang config.yaml

Tệp `openspec/project.md` cũ là một tệp markdown tự do. Tệp `openspec/config.yaml` mới có cấu trúc và—quan trọng hơn—**được đưa vào mọi yêu cầu lập kế hoạch**, do đó các quy ước của bạn luôn hiện diện khi AI làm việc.

### Trước đây (project.md)
```markdown
# Ngữ cảnh Dự án
Sử dụng TypeScript, React và Node.js.
Dùng Jest để test.
```

### Sau này (config.yaml)
```yaml
schema: spec-driven
context: |
  Tech stack: TypeScript, React, Node.js
  Testing: Jest với React Testing Library
rules:
  specs:
    - Sử dụng định dạng Given/When/Then cho các scenarios
```

### Những gì nên giữ, những gì nên bỏ

Khi di chuyển, hãy chọn lọc. Tự hỏi mình: "AI có thực sự cần điều này cho *mọi* yêu cầu lập kế hoạch không?"
- **Giữ trong `context:`**: Tech stack, các mẫu kiến trúc chính, các ràng buộc quan trọng.
- **Chuyển sang `rules:`**: Các hướng dẫn cụ thể cho từng artifact (ví dụ: "dùng Given/When/Then trong specs").
- **Bỏ hoàn toàn**: Các quy tắc chung mà AI đã biết, các giải thích dài dòng hoặc bối cảnh lịch sử không còn ảnh hưởng đến hiện tại.

---

## Các lệnh mới

Sau khi di chuyển, bạn có 9 lệnh OPSX thay vì 3:

| Lệnh | Mục đích |
|---------|---------|
| `/opsx:explore` | Suy nghĩ các ý tưởng không cần cấu trúc |
| `/opsx:new` | Bắt đầu một change mới |
| `/opsx:continue` | Tạo artifact tiếp theo (từng cái một) |
| `/opsx:ff` | Fast-forward—tạo tất cả các artifacts lập kế hoạch cùng lúc |
| `/opsx:apply` | Thực hiện code từ tasks.md |
| `/opsx:verify` | Xác minh implementation khớp với specs |
| `/opsx:archive` | Hoàn tất và lưu trữ change |

---

## Hiểu về Kiến trúc mới

### Từ Phase-Locked sang Fluid (Linh hoạt)
Quy trình cũ bắt buộc tiến triển tuyến tính (Giai đoạn Lập kế hoạch -> Giai đoạn Thực hiện -> Giai đoạn Lưu trữ). OPSX sử dụng các hành động, không phải các giai đoạn. Bạn có thể cập nhật bất kỳ artifact nào vào bất kỳ lúc nào.

### Skills vs Commands
Hệ thống cũ sử dụng các tệp lệnh cụ thể cho từng công cụ (ví dụ: `.claude/commands/`). OPSX sử dụng tiêu chuẩn **skills** mới (ví dụ: `.claude/skills/`), được hỗ trợ rộng rãi bởi nhiều công cụ AI và cung cấp metadata phong phú hơn.

---

## Tiếp tục các Changes hiện có

Các changes đang thực hiện của bạn sẽ hoạt động mượt mà với các lệnh OPSX.
Ví dụ: `/opsx:apply add-my-feature`. OPSX sẽ đọc các artifacts hiện có và tiếp tục từ nơi bạn đã dừng lại.

---

## Giải quyết sự cố

- **Lệnh không xuất hiện**: Hãy khởi động lại IDE của bạn để AI cập nhật các kỹ năng (skills) mới.
- **Config không áp dụng**: Đảm bảo tệp là `openspec/config.yaml` (không phải `.yml`) và kiểm tra cú pháp YAML.
- **project.md chưa được di chuyển**: Hệ thống cố tình giữ lại tệp này để bạn tự xem xét. Hãy xóa nó sau khi đã chuyển nội dung sang `config.yaml`.
