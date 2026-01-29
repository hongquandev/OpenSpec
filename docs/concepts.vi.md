# Khái niệm

Hướng dẫn này giải thích các ý tưởng cốt lõi đằng sau OpenSpec và cách chúng kết hợp với nhau. Để tìm hiểu cách sử dụng thực tế, hãy xem [Bắt đầu](getting-started.md) và [Workflows](workflows.md).

## Triết lý

OpenSpec được xây dựng dựa trên bốn nguyên tắc:

```
fluid not rigid       — linh hoạt không cứng nhắc: không có các cổng giai đoạn (phase gates), làm việc dựa trên những gì hợp lý
iterative not waterfall — lặp lại không theo mô hình thác nước: học hỏi khi xây dựng, tinh chỉnh khi thực hiện
easy not complex      — đơn giản không phức tạp: thiết lập nhẹ nhàng, giảm thiểu các thủ tục rườm rà
brownfield-first      — ưu tiên brownfield: hoạt động tốt với các codebase hiện có, không chỉ dành cho các dự án mới (greenfield)
```

### Tại sao những nguyên tắc này lại quan trọng

**Linh hoạt không cứng nhắc (Fluid not rigid).** Các hệ thống spec truyền thống thường khóa bạn vào các giai đoạn (phases): đầu tiên bạn lập kế hoạch, sau đó bạn thực hiện (implement), và sau đó bạn hoàn thành. OpenSpec linh hoạt hơn — bạn có thể tạo các artifacts theo bất kỳ trình tự nào hợp lý cho công việc của mình.

**Lặp lại không theo mô hình thác nước (Iterative not waterfall).** Yêu cầu (Requirements) thay đổi. Sự hiểu biết sâu sắc hơn. Những gì dường như là một cách tiếp cận tốt lúc đầu có thể không còn phù hợp sau khi bạn tìm hiểu codebase. OpenSpec chấp nhận thực tế này.

**Đơn giản không phức tạp (Easy not complex).** Một số framework về spec yêu cầu thiết lập sâu rộng, định dạng cứng nhắc hoặc các quy trình nặng nề. OpenSpec sẽ không làm phiền bạn. Khởi tạo trong vài giây, bắt đầu làm việc ngay lập tức, và chỉ tùy chỉnh nếu bạn cần.

**Ưu tiên brownfield (Brownfield-first).** Hầu hết các công việc phần mềm không phải là xây dựng từ đầu — mà là sửa đổi các hệ thống hiện có. Cách tiếp cận dựa trên delta của OpenSpec giúp bạn dễ dàng chỉ định các thay đổi đối với hành vi hiện tại, thay vì chỉ mô tả các hệ thống mới.

## Cái nhìn tổng quan

OpenSpec tổ chức công việc của bạn thành hai khu vực chính:

```
┌─────────────────────────────────────────────────────────────────┐
│                        openspec/                                 │
│                                                                  │
│   ┌─────────────────────┐      ┌──────────────────────────────┐ │
│   │       specs/        │      │         changes/              │ │
│   │                     │      │                               │ │
│   │  Nguồn sự thật      │◄─────│  Các đề xuất sửa đổi          │ │
│   │  (Source of truth)  │ merge│  Mỗi change = một thư mục     │ │
│   │  Cách hệ thống hiện │      │  Chứa artifacts + deltas      │ │
│   │  tại đang hoạt động │      │                               │ │
│   └─────────────────────┘      └──────────────────────────────┘ │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Specs** là nguồn sự thật (source of truth) — chúng mô tả cách hệ thống của bạn hiện đang hoạt động.

**Changes** là các đề xuất sửa đổi — chúng nằm trong các thư mục riêng biệt cho đến khi bạn sẵn sàng merge chúng.

Sự tách biệt này là chìa khóa. Bạn có thể làm việc trên nhiều changes song song mà không bị xung đột. Bạn có thể review (đánh giá) một change trước khi nó ảnh hưởng đến các specs chính. Và khi bạn archive (lưu trữ) một change, các deltas của nó sẽ được merge sạch sẽ vào source of truth.

## Specs

Specs mô tả hành vi của hệ thống bằng cách sử dụng các yêu cầu (requirements) và kịch bản (scenarios) có cấu trúc.

### Cấu trúc

```
openspec/specs/
├── auth/
│   └── spec.md           # Hành vi xác thực (Authentication)
├── payments/
│   └── spec.md           # Xử lý thanh toán (Payment processing)
├── notifications/
│   └── spec.md           # Hệ thống thông báo (Notification system)
└── ui/
    └── spec.md           # Hành vi UI và themes
```

Tổ chức specs theo domain — các nhóm logic hợp lý cho hệ thống của bạn. Các mẫu phổ biến:

- **Theo khu vực tính năng**: `auth/`, `payments/`, `search/`
- **Theo component**: `api/`, `frontend/`, `workers/`
- **Theo bounded context**: `ordering/`, `fulfillment/`, `inventory/`

### Định dạng Spec

Một spec chứa các yêu cầu (requirements), và mỗi requirement có các kịch bản (scenarios):

```markdown
# Auth Specification

## Mục đích (Purpose)
Xác thực và quản lý session cho ứng dụng.

## Yêu cầu (Requirements)

### Requirement: User Authentication
Hệ thống SHALL (PHẢI) cấp một JWT token khi đăng nhập thành công.

#### Scenario: Valid credentials
- GIVEN một user với thông tin đăng nhập hợp lệ
- WHEN user gửi form đăng nhập
- THEN một JWT token được trả về
- AND user được chuyển hướng đến dashboard

#### Scenario: Invalid credentials
- GIVEN thông tin đăng nhập không hợp lệ
- WHEN user gửi form đăng nhập
- THEN một thông báo lỗi được hiển thị
- AND không có token nào được cấp
```

**Các yếu tố chính:**

| Yếu tố | Mục đích |
|---------|---------|
| `## Purpose` | Mô tả cấp cao về domain của spec này |
| `### Requirement:` | Một hành vi cụ thể mà hệ thống phải có |
| `#### Scenario:` | Một ví dụ cụ thể về requirement đang hoạt động |
| SHALL/MUST/SHOULD | Các từ khóa RFC 2119 chỉ định mức độ quan trọng của yêu cầu |

### Tại sao lại cấu trúc Specs theo cách này

**Requirements là "cái gì" (what)** — chúng nêu rõ những gì hệ thống nên làm mà không chỉ định cách thực hiện (implementation).

**Scenarios là "khi nào" (when)** — chúng cung cấp các ví dụ cụ thể có thể kiểm chứng được. Các scenarios tốt:
- Có thể kiểm thử được (bạn có thể viết test tự động cho chúng)
- Bao gồm cả trường hợp thông thường (happy path) và các trường hợp biên (edge cases)
- Sử dụng Given/When/Then hoặc định dạng cấu trúc tương tự

**Từ khóa RFC 2119** (SHALL, MUST, SHOULD, MAY) truyền đạt ý định:
- **MUST/SHALL** — yêu cầu bắt buộc tuyệt đối
- **SHOULD** — được khuyến nghị, nhưng có thể có ngoại lệ
- **MAY** — tùy chọn

## Changes

Một change là một đề xuất sửa đổi đối với hệ thống của bạn, được đóng gói dưới dạng một thư mục với mọi thứ cần thiết để hiểu và thực hiện nó.

### Cấu trúc Change

```
openspec/changes/add-dark-mode/
├── proposal.md           # Tại sao và cái gì (Why and what)
├── design.md             # Cách thức (Technical approach)
├── tasks.md              # Danh sách kiểm tra thực hiện (Implementation checklist)
├── .openspec.yaml        # Change metadata (tùy chọn)
└── specs/                # Delta specs
    └── ui/
        └── spec.md       # Những gì thay đổi trong ui/spec.md
```

Mỗi change là độc lập. Nó có:
- **Artifacts** — các tài liệu nắm bắt ý định, thiết kế và các tác vụ (tasks)
- **Delta specs** — đặc tả cho những gì đang được thêm vào, sửa đổi hoặc loại bỏ
- **Metadata** — cấu hình tùy chọn cho change cụ thể này

### Tại sao Changes lại là các thư mục

Việc đóng gói một change dưới dạng thư mục mang lại một số lợi ích:

1. **Mọi thứ tập trung.** Proposal, design, tasks, và specs sống cùng một nơi. Không cần tốn công tìm kiếm ở nhiều vị trí khác nhau.

2. **Làm việc song song.** Nhiều changes có thể tồn tại đồng thời mà không bị xung đột. Bạn có thể làm việc trên `add-dark-mode` trong khi `fix-auth-bug` cũng đang được tiến hành.

3. **Lịch sử sạch sẽ.** Khi được lưu trữ (archived), các changes sẽ di chuyển vào `changes/archive/` với đầy đủ ngữ cảnh được bảo tồn. Bạn có thể nhìn lại và hiểu không chỉ những gì đã thay đổi, mà còn cả lý do tại sao.

4. **Thân thiện với việc Review.** Một thư mục change rất dễ để review — chỉ cần mở nó, đọc proposal, kiểm tra design, và xem các spec deltas.

## Artifacts

Artifacts là các tài liệu trong một change dùng để hướng dẫn công việc.

### Luồng Artifact

```
proposal ──────► specs ──────► design ──────► tasks ──────► implement
    │               │             │              │
   tại sao        cái gì         cách thức     các bước
  + phạm vi      thay đổi       tiếp cận       cần thực hiện
```

Các Artifacts xây dựng dựa trên nhau. Mỗi artifact cung cấp ngữ cảnh cho cái tiếp theo.

### Các loại Artifact

#### Proposal (`proposal.md`)

Proposal nắm bắt **ý định (intent)**, **phạm vi (scope)**, và **cách tiếp cận (approach)** ở mức độ tổng quát.

```markdown
# Proposal: Add Dark Mode

## Intent (Ý định)
Người dùng đã yêu cầu tùy chọn dark mode để giảm mỏi mắt
khi sử dụng vào ban đêm và phù hợp với sở thích của hệ thống.

## Scope (Phạm vi)
Trong phạm vi:
- Nút chuyển đổi giao diện (theme toggle) trong cài đặt
- Phát hiện tùy chọn của hệ thống
- Lưu trữ sở thích trong localStorage

Ngoài phạm vi:
- Các theme màu tùy chỉnh (công việc trong tương lai)
- Ghi đè theme cho từng trang

## Approach (Cách tiếp cận)
Sử dụng CSS custom properties để làm giao diện với một React context
để quản lý state. Phát hiện tùy chọn hệ thống trong lần tải đầu tiên,
cho phép ghi đè thủ công.
```

**Khi nào cần cập nhật proposal:**
- Phạm vi thay đổi (thu hẹp hoặc mở rộng)
- Ý định rõ ràng hơn (hiểu rõ hơn về vấn đề)
- Cách tiếp cận thay đổi về cơ bản

#### Specs (delta specs trong `specs/`)

Delta specs mô tả **những gì đang thay đổi** so với các specs hiện tại. Xem phần [Delta Specs](#delta-specs) bên dưới.

#### Design (`design.md`)

Design nắm bắt **cách tiếp cận kỹ thuật (technical approach)** và các **quyết định kiến trúc (architecture decisions)**.

```markdown
# Design: Add Dark Mode

## Technical Approach
Theme state được quản lý thông qua React Context để tránh prop drilling.
CSS custom properties cho phép chuyển đổi trong thời gian chạy (runtime) mà không cần chuyển đổi class.

## Architecture Decisions (Quyết định kiến trúc)

### Decision: Context over Redux
Sử dụng React Context cho theme state vì:
- Trạng thái nhị phân đơn giản (light/dark)
- Không có các chuyển đổi trạng thái phức tạp
- Tránh việc thêm dependency Redux

### Decision: CSS Custom Properties
Sử dụng biến CSS thay vì CSS-in-JS vì:
- Hoạt động với stylesheet hiện có
- Không có gánh nặng runtime
- Giải pháp trình duyệt gốc (browser-native)
```

**Khi nào cần cập nhật design:**
- Quá trình implementation cho thấy cách tiếp cận sẽ không hoạt động
- Giải pháp tốt hơn được phát hiện
- Các ràng buộc hoặc dependency thay đổi

#### Tasks (`tasks.md`)

Tasks là **danh sách kiểm tra thực hiện (implementation checklist)** — các bước cụ thể với các ô đánh dấu (checkboxes).

```markdown
# Tasks

## 1. Cơ sở hạ tầng Theme (Theme Infrastructure)
- [ ] 1.1 Tạo ThemeContext với trạng thái light/dark
- [ ] 1.2 Thêm CSS custom properties cho màu sắc
- [ ] 1.3 Thực hiện lưu trữ localStorage
- [ ] 1.4 Thêm phát hiện tùy chọn của hệ thống

## 2. Các UI Components
- [ ] 2.1 Tạo component ThemeToggle
- [ ] 2.2 Thêm nút chuyển đổi vào trang cài đặt
- [ ] 2.3 Cập nhật Header để bao gồm nút chuyển đổi nhanh
```

**Các thực hành tốt nhất cho Task:**
- Nhóm các task liên quan dưới các tiêu đề
- Sử dụng đánh số phân cấp (1.1, 1.2, v.v.)
- Giữ các task đủ nhỏ để hoàn thành trong một phiên làm việc
- Đánh dấu các task khi bạn hoàn thành chúng

## Delta Specs

Delta specs là khái niệm then chốt giúp OpenSpec hoạt động tốt cho việc phát triển brownfield. Chúng mô tả **những gì đang thay đổi** thay vì nhắc lại toàn bộ spec.

### Định dạng

```markdown
# Delta cho Auth

## Yêu cầu THÊM MỚI (ADDED)

### Requirement: Two-Factor Authentication
Hệ thống PHẢI (MUST) hỗ trợ xác thực hai yếu tố dựa trên TOTP.

#### Scenario: 2FA enrollment
- GIVEN một user chưa bật 2FA
- WHEN user bật 2FA trong cài đặt
- THEN một mã QR được hiển thị để thiết lập ứng dụng xác thực
- AND user phải xác minh bằng một mã trước khi kích hoạt

## Yêu cầu SỬA ĐỔI (MODIFIED)

### Requirement: Session Expiration
Hệ thống PHẢI (MUST) hết hạn session sau 15 phút không hoạt động.
(Trước đây: 30 phút)

## Yêu cầu LOẠI BỎ (REMOVED)

### Requirement: Remember Me
(Bị loại bỏ để ưu tiên 2FA. Người dùng nên xác thực lại trong mỗi session.)
```

### Các phần của Delta

| Phần | Ý nghĩa | Điều gì xảy ra khi Archive |
|---------|---------|------------------------|
| `## ADDED Requirements` | Hành vi mới | Được thêm vào cuối main spec |
| `## MODIFIED Requirements` | Hành vi thay đổi | Thay thế requirement hiện có |
| `## REMOVED Requirements` | Hành vi không còn sử dụng | Bị xóa khỏi main spec |

### Tại sao lại dùng Deltas thay vì Toàn bộ Specs

**Rõ ràng (Clarity).** Một delta cho thấy chính xác những gì đang thay đổi. Nếu đọc một bản spec đầy đủ, bạn sẽ phải tự so sánh (diff) nó với phiên bản hiện tại trong đầu.

**Tránh xung đột.** Hai thay đổi có thể tác động vào cùng một file spec mà không gây xung đột, miễn là chúng sửa đổi các requirements khác nhau.

**Hiệu quả khi Review.** Người đánh giá chỉ thấy những thay đổi, không phải những phần không thay đổi. Tập trung vào những gì quan trọng.

**Phù hợp với Brownfield.** Hầu hết công việc là sửa đổi hành vi hiện có. Deltas làm cho các sửa đổi trở nên quan trọng hàng đầu, không phải là một ý tưởng nảy ra sau đó.

## Schemas

Schemas định nghĩa các loại artifact và các dependency (phụ thuộc) của chúng cho một workflow.

### Cách thức hoạt động của Schemas

```yaml
# openspec/schemas/spec-driven/schema.yaml
name: spec-driven
artifacts:
  - id: proposal
    generates: proposal.md
    requires: []              # Không phụ thuộc, có thể tạo trước

  - id: specs
    generates: specs/**/*.md
    requires: [proposal]      # Cần proposal trước khi tạo

  - id: design
    generates: design.md
    requires: [proposal]      # Có thể tạo song song với specs

  - id: tasks
    generates: tasks.md
    requires: [specs, design] # Cần cả specs và design trước
```

### Schemas có sẵn

**spec-driven** (mặc định)

Workflow tiêu chuẩn để phát triển theo hướng spec (spec-driven development):

```
proposal → specs → design → tasks → implement
```

Phù hợp cho: Hầu hết các công việc tính năng nơi bạn muốn thống nhất về specs trước khi implementation.

## Archive

Archiving hoàn thành một change bằng cách merge các delta specs của nó vào các specs chính và bảo tồn change đó cho lịch sử.

### Điều gì xảy ra khi bạn Archive

1. **Merge deltas.** Mỗi phần delta spec (ADDED/MODIFIED/REMOVED) được áp dụng cho main spec tương ứng.

2. **Di chuyển vào archive.** Thư mục change sẽ chuyển vào `changes/archive/` với tiền tố ngày tháng để sắp xếp theo trình tự thời gian.

3. **Bảo tồn ngữ cảnh.** Tất cả artifacts vẫn còn nguyên vẹn trong archive. Bạn luôn có thể nhìn lại để hiểu tại sao một thay đổi đã được thực hiện.

### Tại sao Archive lại quan trọng

**Trạng thái sạch sẽ.** Các changes đang hoạt động (`changes/`) chỉ hiển thị những công việc đang được tiến hành. Công việc đã hoàn thành sẽ được chuyển đi chỗ khác.

**Lưu vết kiểm tra (Audit trail).** Archive bảo tồn toàn bộ ngữ cảnh của mọi thay đổi — không chỉ những gì đã thay đổi, mà cả proposal giải thích tại sao, design giải thích cách thức, và các tasks hiển thị công việc đã thực hiện.

**Sự tiến hóa của Spec.** Specs phát triển một cách tự nhiên khi các changes được lưu trữ. Mỗi bản archive merge các deltas của nó, xây dựng nên một đặc tả toàn diện theo thời gian.

## Cách mọi thứ kết hợp với nhau

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              LUỒNG OPENSPEC                                  │
│                                                                              │
│   ┌────────────────┐                                                         │
│   │  1. BẮT ĐẦU    │  /opsx:new tạo một thư mục change                       │
│   │     CHANGE     │                                                         │
│   └───────┬────────┘                                                         │
│           │                                                                  │
│           ▼                                                                  │
│   ┌────────────────┐                                                         │
│   │  2. TẠO        │  /opsx:ff hoặc /opsx:continue                           │
│   │     ARTIFACTS  │  Tạo proposal → specs → design → tasks                  │
│   │                │  (dựa trên các dependency của schema)                   │
│   └───────┬────────┘                                                         │
│           │                                                                  │
│           ▼                                                                  │
│   ┌────────────────┐                                                         │
│   │  3. THỰC HIỆN  │  /opsx:apply                                            │
│   │     TASKS      │  Làm việc thông qua các tasks, đánh dấu hoàn thành      │
│   │                │◄──── Cập nhật artifacts khi bạn hiểu rõ hơn             │
│   └───────┬────────┘                                                         │
│           │                                                                  │
│           ▼                                                                  │
│   ┌────────────────┐                                                         │
│   │  4. XÁC MINH   │  /opsx:verify (tùy chọn)                                │
│   │     CÔNG VIỆC  │  Kiểm tra implementation khớp với specs                 │
│   └───────┬────────┘                                                         │
│           │                                                                  │
│           ▼                                                                  │
│   ┌────────────────┐     ┌──────────────────────────────────────────────┐   │
│   │  5. LƯU TRỮ    │────►│  Delta specs merge vào main specs            │   │
│   │     (ARCHIVE)  │     │  Thư mục change di chuyển vào archive/       │   │
│   └────────────────┘     │  Specs giờ đây là source of truth được cập nhật  │   │
│                          └──────────────────────────────────────────────┘   │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Thuật ngữ (Glossary)

| Thuật ngữ | Định nghĩa |
|------|------------|
| **Artifact** | Một tài liệu trong một change (proposal, design, tasks, hoặc delta specs) |
| **Archive** | Quy trình hoàn thành một change và merge các deltas của nó vào main specs |
| **Change** | Một đề xuất sửa đổi đối với hệ thống, được đóng gói dưới dạng thư mục với artifacts |
| **Delta spec** | Một spec mô tả các thay đổi (ADDED/MODIFIED/REMOVED) so với các specs hiện tại |
| **Domain** | Một nhóm logic cho các specs (ví dụ: `auth/`, `payments/`) |
| **Requirement** | Một hành vi cụ thể mà hệ thống phải có |
| **Scenario** | Một ví dụ cụ thể về một requirement, thường theo định dạng Given/When/Then |
| **Schema** | Một định nghĩa về các loại artifact và các dependency của chúng |
| **Spec** | Một đặc tả mô tả hành vi của hệ thống, chứa các requirements và scenarios |
| **Source of truth** | Thư mục `openspec/specs/`, chứa các hành vi hiện tại đã được thống nhất |

## Các bước tiếp theo

- [Bắt đầu](getting-started.md) - Các bước thực tế đầu tiên
- [Workflows](workflows.md) - Các mẫu phổ biến và khi nào nên sử dụng từng loại
- [Lệnh (Commands)](commands.md) - Tham chiếu lệnh đầy đủ
- [Tùy chỉnh (Customization)](customization.md) - Tạo các schemas tùy chỉnh và cấu hình dự án của bạn
