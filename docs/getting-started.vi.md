# Bắt đầu

Hướng dẫn này giải thích cách OpenSpec hoạt động sau khi bạn đã cài đặt và khởi tạo nó. Để biết hướng dẫn cài đặt, hãy xem [README chính](../README.md#quick-start).

## Cách thức hoạt động

OpenSpec giúp bạn và trợ lý lập trình AI của bạn thống nhất về những gì cần xây dựng trước khi viết bất kỳ đoạn code nào. Quy trình làm việc tuân theo một mẫu đơn giản:

```
┌────────────────────┐
│ Bắt đầu một Change │  /opsx:new
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ Tạo các Artifacts  │  /opsx:ff hoặc /opsx:continue
│ (proposal, specs,  │
│  design, tasks)    │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ Thực hiện Tasks    │  /opsx:apply
│ (AI viết code)     │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ Lưu trữ & Merge    │  /opsx:archive
│ Specs              │
└────────────────────┘
```

## Những gì OpenSpec tạo ra

Sau khi chạy `openspec init`, dự án của bạn có cấu trúc như sau:

```
openspec/
├── specs/              # Nguồn sự thật (Hành vi hệ thống của bạn)
│   └── <domain>/
│       └── spec.md
├── changes/            # Các cập nhật đề xuất (mỗi change một thư mục)
│   └── <change-name>/
│       ├── proposal.md
│       ├── design.md
│       ├── tasks.md
│       └── specs/      # Delta specs (những gì đang thay đổi)
│           └── <domain>/
│               └── spec.md
└── config.yaml         # Cấu hình dự án (tùy chọn)
```

**Hai thư mục chính:**

- **`specs/`** - Nguồn sự thật (Source of truth). Các specs này mô tả cách hệ thống của bạn hiện đang hoạt động. Được tổ chức theo domain (ví dụ: `specs/auth/`, `specs/payments/`).

- **`changes/`** - Các đề xuất sửa đổi. Mỗi change có thư mục riêng với tất cả các artifacts liên quan. Khi một change hoàn thành, các specs của nó sẽ được merge vào thư mục `specs/` chính.

## Hiểu về Artifacts

Mỗi thư mục change chứa các artifacts để hướng dẫn công việc:

| Artifact | Mục đích |
|----------|---------|
| `proposal.md` | "Tại sao" và "Cái gì" - nắm bắt ý định, phạm vi và cách tiếp cận |
| `specs/` | Delta specs hiển thị các yêu cầu THÊM/SỬA/XÓA (ADDED/MODIFIED/REMOVED) |
| `design.md` | "Cách thức" - cách tiếp cận kỹ thuật và các quyết định kiến trúc |
| `tasks.md` | Danh sách kiểm tra thực hiện với các ô đánh dấu |

**Các Artifacts xây dựng dựa trên nhau:**

```
proposal ──► specs ──► design ──► tasks ──► implement
   ▲           ▲          ▲                    │
   └───────────┴──────────┴────────────────────┘
            cập nhật khi bạn hiểu rõ hơn
```

Bạn luôn có thể quay lại và tinh chỉnh các artifacts trước đó khi bạn hiểu rõ hơn trong quá trình thực hiện (implementation).

## Cách Delta Specs hoạt động

Delta specs là khái niệm then chốt trong OpenSpec. Chúng hiển thị những gì đang thay đổi so với các specs hiện tại của bạn.

### Định dạng

Delta specs sử dụng các phần để chỉ định loại thay đổi:

```markdown
# Delta cho Auth

## Yêu cầu THÊM MỚI (ADDED)

### Requirement: Two-Factor Authentication
Hệ thống PHẢI (MUST) yêu cầu yếu tố thứ hai trong quá trình đăng nhập.

#### Scenario: OTP required
- GIVEN một user đã bật 2FA
- WHEN user gửi thông tin đăng nhập hợp lệ
- THEN một yêu cầu OTP được hiển thị

## Yêu cầu SỬA ĐỔI (MODIFIED)

### Requirement: Session Timeout
Hệ thống SHALL (PHẢI) hết hạn session sau 30 phút không hoạt động.
(Trước đây: 60 phút)

#### Scenario: Idle timeout
- GIVEN một authenticated session
- WHEN 30 phút trôi qua mà không có hoạt động
- THEN session bị vô hiệu hóa

## Yêu cầu LOẠI BỎ (REMOVED)

### Requirement: Remember Me
(Bị loại bỏ để ưu tiên 2FA)
```

### Điều gì xảy ra khi Archive

Khi bạn archive một change:

1. Các yêu cầu **ADDED** được thêm vào cuối main spec.
2. Các yêu cầu **MODIFIED** thay thế phiên bản hiện có.
3. Các yêu cầu **REMOVED** bị xóa khỏi main spec.

Thư mục change sẽ chuyển vào `openspec/changes/archive/` để lưu vết lịch sử kiểm tra (audit history).

## Ví dụ: Change đầu tiên của bạn

Hãy cùng đi qua các bước thêm dark mode vào một ứng dụng.

### 1. Bắt đầu Change

```
Bạn: /opsx:new add-dark-mode

AI:  Đã tạo openspec/changes/add-dark-mode/
     Sẵn sàng tạo: proposal
```

### 2. Tạo Artifacts

Sử dụng `/opsx:ff` (fast-forward) để tạo tất cả các artifacts lập kế hoạch cùng một lúc:

```
Bạn: /opsx:ff

AI:  Đang tạo các artifacts cho add-dark-mode...
     ✓ proposal.md — tại sao chúng ta làm điều này, cái gì đang thay đổi
     ✓ specs/       — các yêu cầu và kịch bản
     ✓ design.md    — cách tiếp cận kỹ thuật
     ✓ tasks.md     — danh sách kiểm tra thực hiện
     Sẵn sàng thực hiện!
```

### 3. Những gì được tạo ra

**proposal.md** - Nắm bắt ý định:

```markdown
# Proposal: Add Dark Mode

## Intent
Người dùng đã yêu cầu tùy chọn dark mode để giảm mỏi mắt
khi sử dụng vào ban đêm.

## Scope
- Thêm nút chuyển đổi giao diện trong cài đặt
- Hỗ trợ phát hiện tùy chọn của hệ thống
- Lưu trữ tùy chọn trong localStorage

## Approach
Sử dụng CSS custom properties để làm giao diện với React context
để quản lý state.
```

**specs/ui/spec.md** - Delta hiển thị các yêu cầu mới:

```markdown
# Delta cho UI

## Yêu cầu THÊM MỚI (ADDED)

### Requirement: Theme Selection
Hệ thống SHALL cho phép người dùng chọn giữa giao diện sáng (light) và tối (dark).

#### Scenario: Manual toggle
- GIVEN một người dùng ở bất kỳ trang nào
- WHEN người dùng nhấp vào nút chuyển đổi giao diện
- THEN giao diện chuyển đổi ngay lập tức
- AND tùy chọn được lưu trữ cho các lần truy cập sau
```

**tasks.md** - Danh sách kiểm tra thực hiện:

```markdown
# Tasks

## 1. Cơ sở hạ tầng Theme
- [ ] 1.1 Tạo ThemeContext với trạng thái light/dark
- [ ] 1.2 Thêm CSS custom properties cho màu sắc
- [ ] 1.3 Thực hiện lưu trữ localStorage

## 2. Các UI Components
- [ ] 2.1 Tạo component ThemeToggle
- [ ] 2.2 Thêm nút chuyển đổi vào trang cài đặt
- [ ] 2.3 Cập nhật Header để bao gồm nút chuyển đổi nhanh
```

### 4. Thực hiện (Implement)

```
Bạn: /opsx:apply

AI:  Đang thực hiện các task...
     ✓ 1.1 Đã tạo ThemeContext với trạng thái light/dark
     ✓ 1.2 Đã thêm CSS custom properties vào globals.css
     ✓ 1.3 Đã thực hiện lưu trữ localStorage
     ✓ 2.1 Đã tạo component ThemeToggle
     ...
     Tất cả task đã hoàn thành!
```

Trong quá trình thực hiện, nếu bạn phát hiện design cần điều chỉnh, chỉ cần cập nhật artifact và tiếp tục.

### 5. Lưu trữ (Archive)

```
Bạn: /opsx:archive

AI:  Đang lưu trữ add-dark-mode...
     ✓ Đã merge specs vào openspec/specs/ui/spec.md
     ✓ Đã di chuyển vào openspec/changes/archive/2025-01-24-add-dark-mode/
     Xong! Sẵn sàng cho tính năng tiếp theo.
```

Các delta specs của bạn giờ đây là một phần của main specs, mô tả cách hệ thống của bạn hoạt động.

## Kiểm chứng và Review

Sử dụng CLI để kiểm tra các thay đổi của bạn:

```bash
# Liệt kê các change đang hoạt động
openspec list

# Xem chi tiết một change
openspec show add-dark-mode

# Kiểm tra định dạng spec
openspec validate add-dark-mode

# Dashboard tương tác
openspec view
```

## Các bước tiếp theo

- [Workflows](workflows.md) - Các mẫu phổ biến và khi nào sử dụng mỗi lệnh
- [Commands](commands.md) - Tham chiếu đầy đủ cho tất cả các slash commands
- [Concepts](concepts.md) - Hiểu sâu hơn về specs, changes, và schemas
- [Customization](customization.md) - Làm cho OpenSpec hoạt động theo cách của bạn
