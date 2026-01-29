# Tham chiếu CLI

CLI OpenSpec (`openspec`) cung cấp các lệnh terminal để thiết lập dự án, xác minh, kiểm tra trạng thái và quản lý. Các lệnh này bổ sung cho các lệnh slash commands AI (như `/opsx:new`) được tài liệu hóa trong [Commands](commands.md).

## Tóm tắt

| Danh mục | Lệnh | Mục đích |
|----------|----------|---------|
| **Thiết lập** | `init`, `update` | Khởi tạo và cập nhật OpenSpec trong dự án của bạn |
| **Duyệt** | `list`, `view`, `show` | Khám phá các changes và specs |
| **Xác minh** | `validate` | Kiểm tra lỗi trong các changes và specs |
| **Vòng đời** | `archive` | Hoàn tất các changes đã xong |
| **Workflow** | `status`, `instructions`, `templates`, `schemas` | Hỗ trợ quy trình làm việc theo hướng artifact |
| **Schemas** | `schema init`, `schema fork`, `schema validate`, `schema which` | Tạo và quản lý các workflows tùy chỉnh |
| **Cấu hình** | `config` | Xem và sửa đổi cài đặt |
| **Tiện ích** | `feedback`, `completion` | Phản hồi và tích hợp shell |

---

## Lệnh cho Người dùng vs Agent

Hầu hết các lệnh CLI được thiết kế cho **người dùng** (human use) trong terminal. Một số lệnh cũng hỗ trợ **agent/script use** thông qua đầu ra JSON.

### Lệnh chỉ dành cho Người dùng

Các lệnh này có tính tương tác và được thiết kế để sử dụng trong terminal:

| Lệnh | Mục đích |
|---------|---------|
| `openspec init` | Khởi tạo dự án (có các câu hỏi tương tác) |
| `openspec view` | Dashboard tương tác |
| `openspec config edit` | Mở cấu hình trong trình chỉnh sửa |
| `openspec feedback` | Gửi phản hồi qua GitHub |
| `openspec completion install` | Cài đặt shell completions |

### Lệnh tương thích với Agent

Các lệnh này hỗ trợ đầu ra `--json` để các AI agents và script sử dụng theo chương trình:

| Lệnh | Sử dụng cho Người | Sử dụng cho Agent |
|---------|-----------|-----------|
| `openspec list` | Duyệt các changes/specs | `--json` cho dữ liệu có cấu trúc |
| `openspec show <item>` | Đọc nội dung | `--json` để phân tích cú pháp |
| `openspec validate` | Kiểm tra lỗi | `--all --json` để xác minh hàng loạt |
| `openspec status` | Xem tiến độ artifact | `--json` cho trạng thái có cấu trúc |
| `openspec instructions` | Lấy các bước tiếp theo | `--json` cho các hướng dẫn agent |
| `openspec templates` | Tìm đường dẫn template | `--json` để định vị đường dẫn |
| `openspec schemas` | Liệt kê các schemas có sẵn | `--json` để khám phá schema |

---

## Tùy chọn Toàn cầu

Các tùy chọn này hoạt động với tất cả các lệnh:

| Tùy chọn | Mô tả |
|--------|-------------|
| `--version`, `-V` | Hiển thị số phiên bản |
| `--no-color` | Tắt đầu ra có màu |
| `--help`, `-h` | Hiển thị trợ giúp cho lệnh |

---

## Lệnh Thiết lập

### `openspec init`

Khởi tạo OpenSpec trong dự án của bạn. Tạo cấu trúc thư mục và cấu hình các tích hợp công cụ AI.

```
openspec init [path] [options]
```

**Đối số:**

| Đối số | Bắt buộc | Mô tả |
|----------|----------|-------------|
| `path` | Không | Thư mục đích (mặc định: thư mục hiện tại) |

**Tùy chọn:**

| Tùy chọn | Mô tả |
|--------|-------------|
| `--tools <list>` | Cấu hình các công cụ AI ở chế độ không tương tác. Sử dụng `all`, `none`, hoặc danh sách phân tách bằng dấu phẩy |
| `--force` | Tự động dọn dẹp các tệp cũ mà không cần hỏi lại |

**Các công cụ được hỗ trợ:** `amazon-q`, `antigravity`, `auggie`, `claude`, `cline`, `codex`, `codebuddy`, `continue`, `costrict`, `crush`, `cursor`, `factory`, `gemini`, `github-copilot`, `iflow`, `kilocode`, `opencode`, `qoder`, `qwen`, `roocode`, `windsurf`

**Ví dụ:**

```bash
# Khởi tạo tương tác
openspec init

# Khởi tạo trong một thư mục cụ thể
openspec init ./my-project

# Không tương tác: cấu hình cho Claude và Cursor
openspec init --tools claude,cursor

# Cấu hình cho tất cả các công cụ được hỗ trợ
openspec init --tools all
```

---

### `openspec update`

Cập nhật các tệp hướng dẫn OpenSpec sau khi nâng cấp CLI. Tạo lại các tệp cấu hình công cụ AI.

```
openspec update [path] [options]
```

**Ví dụ:**

```bash
# Cập nhật tệp hướng dẫn sau khi nâng cấp npm
npm update @fission-ai/openspec
openspec update
```

---

## Lệnh Duyệt

### `openspec list`

Liệt kê các changes hoặc specs trong dự án của bạn.

```
openspec list [options]
```

**Tùy chọn:**

| Tùy chọn | Mô tả |
|--------|-------------|
| `--specs` | Liệt kê specs thay vì changes |
| `--changes` | Liệt kê changes (mặc định) |
| `--sort <order>` | Sắp xếp theo `recent` (mặc định) hoặc `name` |
| `--json` | Đầu ra dưới dạng JSON |

---

### `openspec view`

Hiển thị một dashboard tương tác để khám phá các specs và changes.

```
openspec view
```

Mở một giao diện nền terminal để điều hướng các đặc tả và thay đổi trong dự án của bạn.

---

### `openspec show`

Hiển thị thông tin chi tiết của một change hoặc spec.

```
openspec show [item-name] [options]
```

**Tùy chọn:**

| Tùy chọn | Mô tả |
|--------|-------------|
| `--type <type>` | Chỉ định loại: `change` hoặc `spec` |
| `--json` | Đầu ra dưới dạng JSON |
| `--no-interactive` | Tắt các câu hỏi tương tác |

---

## Lệnh Xác minh

### `openspec validate`

Xác minh các changes và specs xem có vấn đề về cấu trúc hay không.

```
openspec validate [item-name] [options]
```

**Tùy chọn:**

| Tùy chọn | Mô tả |
|--------|-------------|
| `--all` | Xác minh tất cả changes và specs |
| `--changes` | Xác minh tất cả changes |
| `--specs` | Xác minh tất cả specs |
| `--strict` | Bật chế độ xác minh nghiêm ngặt |
| `--json` | Đầu ra dưới dạng JSON |
| `--concurrency <n>` | Số lượng xác minh song song tối đa (mặc định: 6) |

---

## Lệnh Vòng đời

### `openspec archive`

Lưu trữ một change đã hoàn thành và merge các delta specs vào main specs.

```
openspec archive [change-name] [options]
```

**Tùy chọn:**

| Tùy chọn | Mô tả |
|--------|-------------|
| `-y, --yes` | Bỏ qua các câu hỏi xác nhận |
| `--skip-specs` | Bỏ qua việc cập nhật spec (cho các thay đổi chỉ về code/doc) |
| `--no-validate` | Bỏ qua việc xác minh (yêu cầu xác nhận) |

---

## Lệnh Workflow

Các lệnh này hỗ trợ quy trình làm việc OPSX theo hướng artifact.

### `openspec status`

Hiển thị trạng thái hoàn thành artifact cho một change.

```
openspec status [options]
```

---

### `openspec instructions`

Lấy các hướng dẫn chi tiết để tạo một artifact hoặc thực hiện các task. Được sử dụng bởi các AI agents để hiểu cần tạo gì tiếp theo.

```
openspec instructions [artifact] [options]
```

---

### `openspec templates`

Hiển thị các đường dẫn template được phân giải cho tất cả các artifacts trong một schema.

```
openspec templates [options]
```

---

### `openspec schemas`

Liệt kê các schemas workflow có sẵn cùng với mô tả và luồng artifact của chúng.

```
openspec schemas [options]
```

---

## Lệnh Schema

Lệnh để tạo và quản lý các workflow schemas tùy chỉnh.

### `openspec schema init`

Tạo một schema cục bộ mới cho dự án.

```
openspec schema init <name> [options]
```

---

### `openspec schema fork`

Sao chép một schema hiện có vào dự án của bạn để tùy chỉnh.

```
openspec schema fork <source> [name] [options]
```

---

### `openspec schema validate`

Xác minh cấu trúc và các templates của một schema.

```
openspec schema validate [name] [options]
```

---

### `openspec schema which`

Hiển thị nơi một schema được phân giải từ đó (hữu ích để gỡ lỗi quyền ưu tiên).

```
openspec schema which [name] [options]
```

**Thứ tự ưu tiên của Schema:**

1. Dự án: `openspec/schemas/<name>/`
2. Người dùng: `~/.local/share/openspec/schemas/<name>/`
3. Gói cài đặt: Các schemas tích hợp sẵn

---

## Lệnh Cấu hình

### `openspec config`

Xem và sửa đổi cấu hình OpenSpec toàn cầu.

```
openspec config <subcommand> [options]
```

**Các lệnh phụ (Subcommands):**

| Lệnh phụ | Mô tả |
|------------|-------------|
| `path` | Hiển thị vị trí tệp cấu hình |
| `list` | Hiển thị tất cả các cài đặt hiện tại |
| `get <key>` | Lấy một giá trị cụ thể |
| `set <key> <value>` | Đặt một giá trị |
| `unset <key>` | Gỡ bỏ một khóa |
| `reset` | Đặt lại về mặc định |
| `edit` | Mở trong trình chỉnh sửa văn bản |

---

## Lệnh Tiện ích

### `openspec feedback`

Gửi phản hồi về OpenSpec. Tạo một GitHub issue.

```
openspec feedback <message> [options]
```

---

### `openspec completion`

Quản lý shell completions cho OpenSpec CLI.

```
openspec completion <subcommand> [shell]
```

---

## Mã thoát (Exit Codes)

| Mã | Ý nghĩa |
|------|---------|
| `0` | Thành công |
| `1` | Lỗi (xác minh thất bại, thiếu file, v.v.) |

---

## Biến môi trường

| Biến | Mô tả |
|----------|-------------|
| `OPENSPEC_CONCURRENCY` | Số lượng song song mặc định cho xác minh hàng loạt |
| `EDITOR` hoặc `VISUAL` | Trình chỉnh sửa cho `openspec config edit` |
| `NO_COLOR` | Tắt đầu ra có màu khi được đặt |

---

## Tài liệu liên quan

- [Commands](commands.md) - Các slash commands AI (`/opsx:new`, `/opsx:apply`, v.v.)
- [Workflows](workflows.md) - Các mẫu phổ biến và khi nào sử dụng từng lệnh
- [Customization](customization.md) - Tạo các schemas và templates tùy chỉnh
- [Getting Started](getting-started.md) - Hướng dẫn thiết lập lần đầu
