---
name: seo-competitor-gap
description: Crawls 1-3 competitor article URLs and compares them against the user's own article to find missing sections, LSI keywords, content depth gaps, format gaps, and E-E-A-T gaps, then produces a prioritized action roadmap. Use when the user provides their own article URL plus one or more competitor URLs and asks to "so sánh đối thủ", "phân tích gap content", or "tối ưu ranking so với đối thủ".
argument-hint: "<own-url> <competitor-url-1> [competitor-url-2] [competitor-url-3]"
license: MIT
user-invokable: true
allowed-tools: WebFetch, Bash(curl:*)
model: inherit
metadata:
  author: anhnhnwork-code
  version: "1.0.0"
  category: seo
---

# seo-competitor-gap

So sánh 1 bài viết của user với tối đa 3 bài đối thủ cùng từ khóa, tìm khoảng trống nội dung, tương tự tính năng "Tối ưu ranking" trong app `seo-optimizer`.

## Input cần có
- URL bài viết của user
- 1–3 URL bài viết đối thủ (cùng từ khóa mục tiêu)

## Các bước

1. **Crawl từng URL** bằng `WebFetch` (ưu tiên) hoặc `curl -s <url>` nếu WebFetch không truy cập được. Trích xuất: heading structure (H1/H2/H3), độ dài nội dung (số từ), danh sách các mục/section chính.

2. **So sánh cấu trúc**
   - Liệt kê các heading/section xuất hiện ở đối thủ nhưng **không có** ở bài của user (missing sections).
   - So sánh độ dài nội dung tổng thể (content depth).
   - Tìm các từ khóa liên quan (LSI keywords) xuất hiện nhiều ở đối thủ nhưng thiếu ở bài user.
   - Đánh giá format: đối thủ có bảng/list/hình ảnh/video mà bài user không có không.
   - Đánh giá tín hiệu E-E-A-T: đối thủ có trích dẫn nguồn, tác giả, ngày cập nhật, số liệu thống kê mà bài user thiếu không.

3. **Xếp ưu tiên** các khoảng trống theo 3 mức: `high` (ảnh hưởng ranking rõ rệt), `medium`, `low`.

4. **Xuất báo cáo roadmap** theo Output Format bên dưới, kèm 1 đoạn rewrite mẫu cho phần thiếu quan trọng nhất.

## Pre-Delivery Review (bắt buộc)
- [ ] Đã crawl thành công tối thiểu bài của user + 1 đối thủ. Nếu tất cả URL đối thủ đều crawl lỗi, báo rõ cho user thay vì tự bịa gap.
- [ ] Mỗi gap nêu ra đều dẫn chứng cụ thể (vd: "đối thủ A có section 'Cách chọn size' mà bài user không có"), không nói chung chung.
- [ ] Roadmap có phân mức ưu tiên rõ ràng (high/medium/low), không liệt kê phẳng.
- [ ] Có ít nhất 1 đoạn rewrite mẫu, không chỉ liệt kê gợi ý suông.

## Error Handling ("vết xe đổ")

| Lỗi | Nguyên nhân | Cách xử lý |
|-----|-------------|------------|
| Không crawl được URL đối thủ | Trang chặn bot / cần JS render | Thử `curl` với proxy như `https://api.allorigins.win/raw?url=<url>` trước khi báo lỗi hẳn |
| Bài viết quá ngắn không đủ so sánh | URL trỏ tới trang danh mục thay vì bài viết | Xác nhận lại URL với user trước khi phân tích |
| So sánh ra quá nhiều gap không thực tế (>20 mục) | Đối thủ thuộc lĩnh vực khác hẳn, không cùng search intent | Dừng, hỏi lại user có đúng đối thủ cùng từ khóa không |
| Không xác định được từ khóa chung | User không nói rõ từ khóa mục tiêu | Suy ra từ khóa từ tiêu đề cả 2 bài, nhưng phải nêu rõ giả định này trong báo cáo |

## Output Format

```
## Competitor Gap Analysis

### Missing Sections (so với đối thủ)
- [high] ...
- [medium] ...

### LSI Keywords thiếu
- ...

### Content Depth
- Bài user: XXX từ | Đối thủ trung bình: XXX từ

### Format Gaps
- ...

### E-E-A-T Gaps
- ...

### Rewrite mẫu (phần ưu tiên cao nhất)
> ...
```
