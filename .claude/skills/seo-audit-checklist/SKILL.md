---
name: seo-audit-checklist
description: Audits a Vietnamese SEO article against the 19-criteria SEONGON rule-based checklist (H1, meta title/description, sapo, formatting, headings, keyword density, images, links, ending paragraph). Use when the user pastes article content or a URL and asks to "chấm SEO", "audit bài viết", "kiểm tra chuẩn SEO", or "check checklist SEONGON".
argument-hint: "<url-or-pasted-text> <từ-khóa-chính>"
license: MIT
user-invokable: true
allowed-tools: Read, WebFetch, Bash(curl:*)
model: inherit
metadata:
  author: anhnhnwork-code
  version: "1.0.0"
  category: seo
---

# seo-audit-checklist

Chấm điểm 1 bài viết SEO theo checklist rule-based chuẩn SEONGON — không cần gọi AI, chỉ cần phân tích cấu trúc/text.

## Input cần có
- Nội dung bài viết: URL (crawl qua WebFetch/curl) hoặc text được paste trực tiếp
- Từ khóa chính của bài (bắt buộc). Nếu user chưa cung cấp, **hỏi lại trước khi chấm điểm** — không tự đoán từ khóa.

## Các bước

1. **Lấy nội dung bài viết**
   - Nếu là URL: dùng `WebFetch` (hoặc `curl -s <url>`) để lấy HTML, trích xuất: H1, title tag, meta description, các đoạn văn, heading H2/H3, danh sách UL/LI, ảnh + ALT, internal/external link.
   - Nếu là text paste: parse trực tiếp theo markdown/HTML đơn giản mà user cung cấp.

2. **Đọc bảng tiêu chí**
   - Đọc file `rules/seo-criteria.md` (cùng thư mục với SKILL.md này) để lấy danh sách đầy đủ 20 tiêu chí và ngưỡng đạt. **Chỉ đọc file này khi thực sự bắt đầu chấm điểm**, không cần đọc trước.

3. **Chấm từng tiêu chí**
   - Với mỗi tiêu chí trong bảng, đánh giá `pass` / `warn` / `fail` dựa theo ngưỡng đã định nghĩa.
   - Ghi chú ngắn gọn lý do (vd: "H1 dài 78 ký tự, vượt ngưỡng 65").

4. **Tính điểm tổng** theo công thức trong `rules/seo-criteria.md` (pass=5, warn=2.5, fail=0, quy về thang 100).

5. **Xuất báo cáo** theo đúng Output Format bên dưới.

## Pre-Delivery Review (bắt buộc, tự kiểm trước khi trả kết quả cho user)
- [ ] Đã chấm đủ toàn bộ 20 tiêu chí trong `rules/seo-criteria.md`, không bỏ sót mục nào.
- [ ] Mỗi tiêu chí có trạng thái pass/warn/fail rõ ràng kèm lý do ngắn.
- [ ] Điểm tổng được tính đúng công thức, không tự bịa số.
- [ ] Nếu không lấy được nội dung bài viết (crawl lỗi), **không** chấm bừa — báo lỗi rõ cho user thay vì trả điểm giả.
- [ ] Nếu thiếu từ khóa chính, đã hỏi lại user trước khi chấm.

## Error Handling ("vết xe đổ")

| Lỗi | Nguyên nhân | Cách xử lý |
|-----|-------------|------------|
| Không lấy được nội dung từ URL | Trang chặn crawl / cần JS render | Báo user, xin họ paste nội dung trực tiếp dạng text/markdown |
| Không xác định được từ khóa chính | User chưa cung cấp | Hỏi lại từ khóa trước khi chấm, không tự đoán |
| Không tìm thấy heading H1/H2 | Bài viết dùng format khác chuẩn (vd chỉ có `**text**` thay vì heading) | Ghi rõ giả định khi parse, note trong báo cáo là "định dạng không chuẩn, kết quả có thể không chính xác 100%" |
| Mật độ từ khóa tính ra > 10% hoặc = 0% | Đếm sai do từ khóa xuất hiện trong code/link, hoặc bài quá ngắn | Kiểm tra lại bằng cách đếm thủ công 1 đoạn mẫu trước khi kết luận |
| File `rules/seo-criteria.md` không đọc được | Chạy skill ở sai project, thiếu file bổ trợ | Báo lỗi rõ, không tự bịa ngưỡng tiêu chí |

## Output Format

```
SEO Audit Score: XX/100

| # | Tiêu chí | Trạng thái | Ghi chú |
|---|----------|-----------|---------|
| 1 | H1 < 65 ký tự | pass/warn/fail | ... |
...(đủ 20 dòng)

Vấn đề ưu tiên cao (fail):
- ...

Vấn đề cần cải thiện (warn):
- ...

Gợi ý chỉnh sửa nhanh:
- ...
```
