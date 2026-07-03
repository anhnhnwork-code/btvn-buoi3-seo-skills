---
name: seo-ai-review
description: Calls the Anthropic Messages API directly via curl to run an AI-based review of a Vietnamese SEO article — spelling/grammar check, title insight, meta description insight, and content improvement suggestions. Use when the user asks for "phân tích AI", "review bằng AI", "gợi ý cải thiện bài viết bằng AI", or wants a second opinion beyond the rule-based checklist.
argument-hint: "<url-or-pasted-text>"
license: MIT
user-invokable: true
allowed-tools: Bash(curl:*), Read
model: inherit
metadata:
  author: anhnhnwork-code
  version: "1.0.0"
  category: seo
---

# seo-ai-review

Gọi trực tiếp Anthropic API (nền tảng ngoài, qua CLI `curl`) để chạy phân tích AI cho bài viết SEO, tương tự tính năng "Phân tích AI" trong app `seo-optimizer`.

## Yêu cầu trước khi chạy
- Biến môi trường `ANTHROPIC_API_KEY` phải được set sẵn trong shell (không hardcode key vào skill hay commit lên git).
- Kiểm tra: `echo $ANTHROPIC_API_KEY` — nếu rỗng, dừng lại và báo user cách set: `export ANTHROPIC_API_KEY="sk-ant-..."`.

## Các bước

1. **Kiểm tra API key**
   ```bash
   test -n "$ANTHROPIC_API_KEY" && echo "OK" || echo "MISSING"
   ```
   Nếu `MISSING`, dừng và yêu cầu user set key trước.

2. **Lấy nội dung bài viết** (URL qua `curl -s <url>` hoặc text user paste trực tiếp).

3. **Gọi Anthropic Messages API**
   ```bash
   curl -s https://api.anthropic.com/v1/messages \
     -H "x-api-key: $ANTHROPIC_API_KEY" \
     -H "anthropic-version: 2023-06-01" \
     -H "content-type: application/json" \
     -d '{
       "model": "claude-sonnet-4-6",
       "max_tokens": 2000,
       "messages": [{
         "role": "user",
         "content": "Bạn là chuyên gia SEO SEONGON. Phân tích bài viết sau và trả lời đúng 4 mục: 1) Lỗi chính tả/ngữ pháp, 2) Insight về tiêu đề, 3) Insight về mô tả SEO, 4) Gợi ý cải thiện nội dung. Bài viết: <NỘI DUNG BÀI VIẾT>"
       }]
     }'
   ```
   Thay `<NỘI DUNG BÀI VIẾT>` bằng nội dung thật, escape ký tự đặc biệt đúng chuẩn JSON.

4. **Parse response** — lấy `content[0].text` từ JSON trả về.

5. **Xuất báo cáo** theo Output Format bên dưới.

## Pre-Delivery Review (bắt buộc)
- [ ] Đã xác nhận `ANTHROPIC_API_KEY` tồn tại trước khi gọi API, không gọi mù.
- [ ] Request JSON hợp lệ (không lỗi escape ký tự khiến curl fail).
- [ ] Response có đủ 4 mục: chính tả, tiêu đề, meta description, gợi ý cải thiện — nếu AI trả thiếu mục nào, gọi lại hoặc bổ sung rõ trong báo cáo là "thiếu mục X".
- [ ] Không bịa nội dung AI không trả về — chỉ trình bày đúng những gì API trả lời.

## Error Handling ("vết xe đổ")

| Lỗi | Nguyên nhân | Cách xử lý |
|-----|-------------|------------|
| HTTP 401 | `ANTHROPIC_API_KEY` sai hoặc chưa set | Yêu cầu user `export ANTHROPIC_API_KEY="sk-ant-..."` rồi thử lại |
| HTTP 429 | Gọi quá nhanh, vượt rate limit | Đợi vài giây rồi retry 1 lần |
| HTTP 400 invalid JSON | Nội dung bài viết chứa ký tự đặc biệt (dấu `"`, xuống dòng) chưa escape | Escape lại chuỗi JSON trước khi gửi, hoặc ghi payload ra file tạm và dùng `curl -d @file.json` |
| Response bị cắt giữa chừng | `max_tokens` quá thấp so với độ dài bài viết | Tăng `max_tokens` lên 4000 và gọi lại |
| `model not found` | Model ID sai hoặc đã deprecated | Kiểm tra lại model ID hiện hành trước khi gọi, báo user nếu cần đổi model |

## Output Format

```
## AI Review Report

### 1. Chính tả & ngữ pháp
- ...

### 2. Insight tiêu đề
- ...

### 3. Insight mô tả SEO
- ...

### 4. Gợi ý cải thiện nội dung
- ...
```
