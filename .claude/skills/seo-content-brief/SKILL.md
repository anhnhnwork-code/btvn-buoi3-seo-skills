---
name: seo-content-brief
description: Tạo content brief / dàn ý chi tiết cho một bài viết SEO mới hoặc bản cải thiện, dựa trên từ khóa mục tiêu và (nếu có) kết quả gap analysis từ skill seo-competitor-gap. Use when the user asks to "tạo content brief", "lên outline bài viết SEO", "viết dàn ý", or after a competitor gap analysis when they want a concrete plan to close the gaps found.
argument-hint: "<từ-khóa-chính> [đường-dẫn-hoặc-nội-dung-gap-analysis]"
license: MIT
user-invokable: true
allowed-tools: Read, WebFetch
model: inherit
metadata:
  author: anhnhnwork-code
  version: "1.0.0"
  category: seo
---

# seo-content-brief

Sinh content brief (dàn ý + hướng dẫn viết) cho 1 bài viết SEO, dùng làm bước tiếp theo sau khi đã có `seo-competitor-gap` (không bắt buộc — có thể chạy độc lập chỉ với từ khóa).

## Input cần có
- Từ khóa chính (bắt buộc). Nếu thiếu, **hỏi lại trước khi tạo brief**.
- Kết quả gap analysis (tuỳ chọn): đường dẫn file trong `outputs/` hoặc nội dung được paste trực tiếp từ output của skill `seo-competitor-gap`.

## Các bước

1. **Xác định từ khóa & search intent** — suy ra người đọc đang muốn gì (so sánh, hướng dẫn, review, mua hàng...) từ từ khóa.
2. **Đọc gap analysis nếu có** — nếu user cung cấp file/nội dung gap analysis, đọc bằng `Read`/nội dung paste để lấy: missing sections, LSI keywords thiếu, content depth, format gaps, E-E-A-T gaps. Nếu không có, bỏ qua bước này và note rõ trong output là "brief dựa thuần trên từ khóa, chưa có dữ liệu đối thủ".
3. **Xây dàn ý** — đề xuất H1, SEO title, meta description, và outline H2/H3 đầy đủ. Với mỗi mục lấy từ gap analysis (phần đối thủ có mà thiếu), đánh dấu rõ nguồn gốc.
4. **Gắn LSI keyword & độ dài mục tiêu** cho từng phần, và liệt kê tín hiệu E-E-A-T cần bổ sung (tác giả, nguồn trích dẫn, số liệu, ngày cập nhật).
5. **Xếp ưu tiên triển khai** theo high/medium/low, ưu tiên các phần ảnh hưởng ranking rõ rệt trước.
6. **Xuất báo cáo** theo đúng Output Format bên dưới.

## Pre-Delivery Review (bắt buộc)
- [ ] Đã có từ khóa chính rõ ràng, không tự suy đoán khi chưa được cung cấp.
- [ ] Nếu có gap analysis, mọi mục outline lấy từ đó đều ghi rõ nguồn ("theo gap analysis với đối thủ X").
- [ ] Dàn ý có đủ H1 + SEO title + meta description + outline H2/H3, không chỉ liệt kê chủ đề chung chung.
- [ ] Mỗi phần outline có LSI keyword hoặc ghi chú viết đi kèm, không để trống.
- [ ] Có phân mức ưu tiên triển khai (high/medium/low).

## Error Handling ("vết xe đổ")

| Lỗi | Nguyên nhân | Cách xử lý |
|-----|-------------|------------|
| Không có từ khóa chính | User quên cung cấp | Hỏi lại trước khi tạo brief, không tự bịa từ khóa |
| File gap analysis không đọc được | Sai đường dẫn / file không tồn tại | Báo lỗi rõ, hỏi user paste trực tiếp nội dung hoặc tạo brief thuần từ khóa (nêu rõ giả định) |
| Gap analysis không liên quan tới từ khóa hiện tại | User đưa nhầm file cũ | Xác nhận lại với user trước khi dùng dữ liệu đó |
| Outline quá sơ sài (dưới 5 mục H2) | Từ khóa quá hẹp hoặc thiếu ngữ cảnh | Hỏi thêm ngữ cảnh (đối tượng đọc, mục đích bài) trước khi hoàn thiện |

## Output Format

```
## Content Brief: <từ khóa>

### Thông tin chung
- Từ khóa chính: ...
- Search intent: ...
- Đối tượng đọc: ...
- Độ dài mục tiêu: ~XXXX từ
- Nguồn dữ liệu: [có/không có gap analysis đối thủ]

### Đề xuất Tiêu đề & Meta
- H1: ...
- SEO Title: ...
- Meta Description: ...

### Dàn ý nội dung
1. Sapo — ...
2. H2: ...
   - H3: ...
   - LSI keyword: ...
   - Ghi chú: (nguồn: gap analysis / tự đề xuất)
...

### E-E-A-T & Format cần bổ sung
- ...

### Ưu tiên triển khai
- [high] ...
- [medium] ...
- [low] ...
```
