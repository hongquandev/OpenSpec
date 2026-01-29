# Các Công cụ được Hỗ trợ

OpenSpec hoạt động với hơn 20 trợ lý lập trình AI. Khi bạn chạy `openspec init`, bạn sẽ được nhắc chọn công cụ mình sử dụng và OpenSpec sẽ cấu hình các tích hợp thích hợp.

## Cách thức hoạt động

Đối với mỗi công cụ bạn chọn, OpenSpec sẽ cài đặt:

1. **Skills** — Các tệp hướng dẫn có thể tái sử dụng để vận hành các lệnh quy trình `/opsx:*`.
2. **Commands** — Các liên kết lệnh slash command cụ thể cho từng công cụ.

## Tham chiếu Danh mục Công cụ

| Công cụ | Vị trí Skills | Vị trí Lệnh |
|------|-----------------|-------------------|
| Amazon Q Developer | `.amazonq/skills/` | `.amazonq/prompts/` |
| Antigravity | `.agent/skills/` | `.agent/workflows/` |
| Claude Code | `.claude/skills/` | `.claude/commands/opsx/` |
| Cursor | `.cursor/skills/` | `.cursor/commands/` |
| Gemini CLI | `.gemini/skills/` | `.gemini/commands/opsx/` |
| GitHub Copilot | `.github/skills/` | `.github/prompts/` |
| Windsurf | `.windsurf/skills/` | `.windsurf/commands/opsx/` |
| *Và các công cụ khác...* | | |

## Thiết lập Không tương tác

Đối với CI/CD hoặc thiết lập bằng script, hãy sử dụng cờ `--tools`:

```bash
# Cấu hình các công cụ cụ thể
openspec init --tools claude,cursor

# Cấu hình tất cả các công cụ được hỗ trợ
openspec init --tools all

# Bỏ qua cấu hình công cụ
openspec init --tools none
```

**Các tool IDs có sẵn:** `amazon-q`, `antigravity`, `auggie`, `claude`, `cline`, `codebuddy`, `codex`, `continue`, `costrict`, `crush`, `cursor`, `factory`, `gemini`, `github-copilot`, `iflow`, `kilocode`, `opencode`, `qoder`, `qwen`, `roocode`, `windsurf`

## Những gì được cài đặt

Đối với mỗi công cụ, OpenSpec tạo ra 10 tệp skill giúp vận hành quy trình OPSX:

| Skill | Mục đích |
|-------|---------|
| `openspec-explore` | Cộng sự cùng suy nghĩ để khám phá ý tưởng |
| `openspec-new-change` | Bắt đầu một change mới |
| `openspec-continue-change` | Tạo artifact tiếp theo |
| `openspec-ff-change` | Tiến nhanh (Fast-forward) qua tất cả các planning artifacts |
| `openspec-apply-change` | Thực hiện các task (Implementation) |
| `openspec-verify-change` | Xác minh tính đầy đủ của việc thực hiện |
| `openspec-sync-specs` | Đồng bộ các delta specs vào bản chính |
| `openspec-archive-change` | Lưu trữ một change đã hoàn tất |
| `openspec-bulk-archive-change` | Lưu trữ nhiều changes cùng lúc |
| `openspec-onboard` | Hướng dẫn qua một chu kỳ quy trình đầy đủ |

Các kỹ năng này được gọi qua các lệnh slash commands như `/opsx:new`, `/opsx:apply`, v.v. Xem [Commands](commands.md) để biết danh sách đầy đủ.

---

## Liên quan

- [Tham chiếu CLI](cli.md) — Các lệnh terminal.
- [Các Lệnh (Commands)](commands.md) — Slash commands và skills.
- [Bắt đầu (Getting Started)](getting-started.md) — Hướng dẫn thiết lập lần đầu.
