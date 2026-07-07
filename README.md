# BTVN Buổi 3 & 4 — SEO Skills & Agents Workspace

Bài tập về nhà Buổi 3 (khóa Claude Code, SEONGON): xây workspace cá nhân với các Claude Skills phục vụ tối ưu bài viết SEO, đồng bộ với logic của app [seo-optimizer](https://github.com/anhnhnwork-code/seo-optimizer).

Bài tập về nhà Buổi 4: nâng cấp workspace bằng 2 sub agent (mỗi agent ≥2 skill), rồi giao 1 nhiệm vụ lớn để Claude Code tự phân bổ cho sub agent phù hợp — xem [phần Buổi 4](#buổi-4--sub-agents) bên dưới.

## Skills

| Skill | Mô tả | Kết nối ngoài | File bổ trợ |
|-------|-------|:---:|:---:|
| [`seo-audit-checklist`](.claude/skills/seo-audit-checklist/SKILL.md) | Chấm điểm bài viết theo 19 tiêu chí rule-based SEONGON | – | ✅ `rules/seo-criteria.md` |
| [`seo-ai-review`](.claude/skills/seo-ai-review/SKILL.md) | Gọi Anthropic API qua curl để review AI (chính tả, tiêu đề, meta, gợi ý) | ✅ Anthropic API | – |
| [`seo-competitor-gap`](.claude/skills/seo-competitor-gap/SKILL.md) | So sánh gap content với 1-3 URL đối thủ | ✅ crawl qua curl/WebFetch | – |

## Cách dùng
Gọi trực tiếp trong Claude Code:
```
/seo-audit-checklist <url-hoặc-text> <từ-khóa-chính>
/seo-ai-review <url-hoặc-text>
/seo-competitor-gap <own-url> <competitor-url-1> [competitor-url-2] [competitor-url-3]
```

`seo-ai-review` cần biến môi trường `ANTHROPIC_API_KEY` đã được set sẵn trong shell.

## Đã chạy thử (xem `outputs/`)
- `2026-07-03-seo-audit-osakar-xe-may-dien.md` — chấm bài https://osakar.com.vn/tin-tuc/nen-mua-xe-may-dien-hang-nao/, đạt 82.5/100
- `2026-07-04-seo-competitor-gap-osakar-vs-vinfast.md` — so gap bài Osakar với 1 bài so sánh xe máy điện VinFast
- `seo-ai-review` chưa chạy thử — cần `ANTHROPIC_API_KEY` cá nhân, tự chạy khi cần (xem lưu ý bảo mật bên dưới)

> **Lưu ý bảo mật:** Không paste API key thật vào cửa sổ chat Claude Code, vì nội dung chat sẽ bị xuất ra file `.txt` khi chạy `/export` và có thể lộ key nếu file đó được đẩy lên GitHub. Hãy set biến môi trường trong terminal của bạn trước khi gọi skill.

## Cấu trúc
```
.claude/skills/
  seo-audit-checklist/
    SKILL.md
    rules/seo-criteria.md
  seo-ai-review/
    SKILL.md
  seo-competitor-gap/
    SKILL.md
  seo-content-brief/
    SKILL.md
.claude/agents/
  seo-onpage-auditor.md
  seo-strategy-planner.md
outputs/       # kết quả chạy thử các skill / agent
conv/          # lịch sử chat xuất bằng /export
```

## Buổi 4 — Sub agents

Nâng cấp workspace buổi 3 bằng 2 sub agent, mỗi agent phụ trách 1 giai đoạn của quy trình SEO và dùng ≥2 skill:

| Agent | Nhiệm vụ | Skills dùng |
|-------|----------|--------------|
| [`seo-onpage-auditor`](.claude/agents/seo-onpage-auditor.md) | Audit & review chất lượng on-page của bài viết **hiện có** | `seo-audit-checklist` (rule-based) + `seo-ai-review` (AI review) |
| [`seo-strategy-planner`](.claude/agents/seo-strategy-planner.md) | Phân tích đối thủ & lên kế hoạch nội dung **mới/cải thiện** | `seo-competitor-gap` (gap analysis) + `seo-content-brief` (dàn ý, mới ở buổi 4) |

### Nhiệm vụ lớn đã giao cho Claude Code tự phân bổ

```
Tối ưu toàn diện bài viết https://osakar.com.vn/tin-tuc/nen-mua-xe-may-dien-hang-nao/
(từ khóa: xe máy điện) để cạnh tranh với 1 bài so sánh xe máy điện của VinFast:
1. Đánh giá chất lượng on-page hiện tại của bài (rule-based + AI review).
2. Phân tích khoảng trống nội dung so với đối thủ VinFast và đề xuất content brief
   cải thiện, ưu tiên các phần ảnh hưởng ranking rõ nhất.
Không tự làm cả 2 phần một mình — hãy phân bổ cho các sub agent phù hợp trong
.claude/agents/ rồi tổng hợp kết quả.
```

Claude Code (orchestrator) đọc `description` của từng agent và tự gọi `seo-onpage-auditor` cho phần (1), `seo-strategy-planner` cho phần (2), rồi gộp kết quả — không cần chỉ định thủ công tên agent trong prompt.

Kết quả chạy thật: xem `outputs/` (file buổi 4) và lịch sử `/export` trong `conv/`.
