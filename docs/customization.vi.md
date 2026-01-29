# Tùy chỉnh (Customization)

OpenSpec cung cấp ba cấp độ tùy chỉnh:

| Cấp độ | Nó làm gì | Phù hợp nhất cho |
|-------|--------------|----------|
| **Cấu hình Dự án** | Đặt mặc định, chèn ngữ cảnh/quy tắc | Hầu hết các nhóm |
| **Schemas Tùy chỉnh** | Định nghĩa các workflow artifact của riêng bạn | Các nhóm có quy trình độc đáo |
| **Ghi đè Toàn cầu** | Chia sẻ schemas trên tất cả các dự án | Người dùng nâng cao |

---

## Cấu hình Dự án

Tệp `openspec/config.yaml` là cách dễ nhất để tùy chỉnh OpenSpec cho nhóm của bạn. Nó cho phép bạn:

- **Đặt một schema mặc định** - Bỏ qua `--schema` cho mỗi lệnh.
- **Chèn ngữ cảnh dự án** - AI sẽ thấy tech stack, quy ước, v.v. của bạn.
- **Thêm quy tắc cho từng artifact** - Quy tắc tùy chỉnh cho các artifacts cụ thể.

### Thiết lập Nhanh

```bash
openspec init
```

Lệnh này sẽ hướng dẫn bạn tạo cấu hình một cách tương tác. Hoặc bạn có thể tạo thủ công:

```yaml
# openspec/config.yaml
schema: spec-driven

context: |
  Tech stack: TypeScript, React, Node.js, PostgreSQL
  API style: RESTful, documented in docs/api.md
  Testing: Jest + React Testing Library
  Chúng tôi coi trọng khả năng tương thích ngược cho tất cả các public APIs.

rules:
  proposal:
    - Bao gồm kế hoạch rollback
    - Xác định các nhóm bị ảnh hưởng
  specs:
    - Sử dụng định dạng Given/When/Then
    - Tham chiếu các mẫu hiện có trước khi tạo mẫu mới
```

### Cách thức hoạt động

**Ngữ cảnh (Context) và Quy tắc (Rules):**

Khi tạo bất kỳ artifact nào, ngữ cảnh và quy tắc của bạn sẽ được chèn vào AI prompt:

- **Context** xuất hiện trong TẤT CẢ các artifacts.
- **Rules** CHỈ xuất hiện cho artifact khớp với tên.

---

## Schemas Tùy chỉnh

Khi cấu hình dự án là không đủ, hãy tạo schema của riêng bạn với một workflow tùy chỉnh hoàn toàn. Các schemas tùy chỉnh nằm trong thư mục `openspec/schemas/` của dự án và được quản lý phiên bản cùng với code của bạn.

### Fork một Schema hiện có

Cách nhanh nhất để tùy chỉnh là fork một schema tích hợp sẵn:

```bash
openspec schema fork spec-driven my-workflow
```

Thao tác này sẽ sao chép toàn bộ schema `spec-driven` vào `openspec/schemas/my-workflow/` để bạn có thể tự do chỉnh sửa.

**Những gì bạn nhận được:**

```text
openspec/schemas/my-workflow/
├── schema.yaml           # Định nghĩa Workflow
└── templates/
    ├── proposal.md       # Template cho proposal
    ├── spec.md           # Template cho specs
    ├── design.md         # Template cho design
    └── tasks.md          # Template cho tasks
```

### Tạo một Schema từ đầu

Để có một workflow hoàn toàn mới:

```bash
# Tương tác
openspec schema init research-first

# Không tương tác
openspec schema init rapid \
  --description "Workflow lặp nhanh" \
  --artifacts "proposal,tasks" \
  --default
```

### Cấu trúc Schema

Một schema định nghĩa các artifacts trong workflow của bạn và cách chúng phụ thuộc lẫn nhau:

```yaml
# openspec/schemas/my-workflow/schema.yaml
name: my-workflow
version: 1
description: Workflow tùy chỉnh của nhóm tôi

artifacts:
  - id: proposal
    generates: proposal.md
    description: Tài liệu đề xuất ban đầu
    template: proposal.md
    instruction: |
      Tạo một bản proposal giải thích TẠI SAO sự thay đổi này là cần thiết.
    requires: []

  - id: design
    generates: design.md
    description: Thiết kế kỹ thuật
    template: design.md
    instruction: |
      Tạo tài liệu design giải thích CÁCH THỰC HIỆN.
    requires:
      - proposal
```

### Xác minh Schema của bạn

Trước khi sử dụng một schema tùy chỉnh, hãy xác minh nó:

```bash
openspec schema validate my-workflow
```

---

## Các ví dụ

### Workflow Lặp nhanh (Rapid Iteration)

Một workflow tối giản cho các lần lặp nhanh:

```yaml
# openspec/schemas/rapid/schema.yaml
name: rapid
version: 1
description: Lặp nhanh với gánh nặng tối thiểu

artifacts:
  - id: proposal
    generates: proposal.md
    requires: []

  - id: tasks
    generates: tasks.md
    requires: [proposal]

apply:
  requires: [tasks]
  tracks: tasks.md
```

---

## Xem thêm

- [Tham chiếu CLI: Lệnh Schema](cli.md#schema-commands) - Tài liệu hướng dẫn đầy đủ về các lệnh.
