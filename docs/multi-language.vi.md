# Hướng dẫn Đa ngôn ngữ (Multi-Language)

Cấu hình OpenSpec để tạo các artifacts bằng các ngôn ngữ khác ngoài Tiếng Anh.

## Thiết lập nhanh

Thêm hướng dẫn ngôn ngữ vào tệp `openspec/config.yaml` của bạn:

```yaml
schema: spec-driven

context: |
  Ngôn ngữ: Tiếng Việt
  Tất cả các artifacts phải được viết bằng Tiếng Việt.

  # Ngữ cảnh dự án khác của bạn bên dưới...
  Tech stack: TypeScript, React, Node.js
```

Vậy là xong. Tất cả các artifacts được tạo ra bây giờ sẽ bằng Tiếng Việt.

## Ví dụ về Ngôn ngữ

### Tiếng Việt

```yaml
context: |
  Ngôn ngữ: Tiếng Việt
  Tất cả các artifacts phải được viết bằng Tiếng Việt.
  Giữ nguyên các thuật ngữ IT chuyên ngành bằng Tiếng Anh (ví dụ: API, Middleware, v.v.).
```

### Tiếng Tây Ban Nha

```yaml
context: |
  Idioma: Español
  Todos los artefactos deben escribirse en español.
```

## Lời khuyên

### Xử lý Thuật ngữ Kỹ thuật

Quyết định cách xử lý các thuật ngữ chuyên môn:

```yaml
context: |
  Ngôn ngữ: Tiếng Việt
  Viết bằng Tiếng Việt, nhưng:
  - Giữ các thuật ngữ kỹ thuật như "API", "REST", "GraphQL" bằng Tiếng Anh.
  - Các ví dụ mã (code examples) và đường dẫn tệp phải giữ nguyên bằng Tiếng Anh.
```

### Kết hợp với Ngữ cảnh khác

Cài đặt ngôn ngữ hoạt động song song với các ngữ cảnh dự án khác của bạn:

```yaml
schema: spec-driven

context: |
  Ngôn ngữ: Tiếng Việt
  Tất cả các artifacts phải được viết bằng Tiếng Việt.

  Tech stack: TypeScript, React 18, Node.js 20
  Database: PostgreSQL với Prisma ORM
```

## Xác minh

Để xác minh cấu hình ngôn ngữ của bạn đang hoạt động:

```bash
# Kiểm tra hướng dẫn - nó sẽ hiển thị ngữ cảnh ngôn ngữ của bạn
openspec instructions proposal --change my-change
```

## Tài liệu liên quan

- [Hướng dẫn Tùy chỉnh](./customization.md) - Các tùy chọn cấu hình dự án.
- [Hướng dẫn Workflows](./workflows.md) - Tài liệu đầy đủ về quy trình làm việc.
