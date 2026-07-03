# BTVN Buổi 3 — SEO Skills Workspace

Bài tập về nhà Buổi 3 (khóa Claude Code, SEONGON): xây workspace cá nhân với các Claude Skills phục vụ tối ưu bài viết SEO, đồng bộ với logic của app [seo-optimizer](https://github.com/anhnhnwork-code/seo-optimizer).

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
outputs/       # kết quả chạy thử các skill
conv/          # lịch sử chat xuất bằng /export
```
