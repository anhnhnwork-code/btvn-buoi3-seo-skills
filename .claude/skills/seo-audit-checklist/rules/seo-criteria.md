# Bảng 19 tiêu chí SEO chuẩn SEONGON

Dùng bảng này để chấm điểm bài viết. Mỗi tiêu chí: pass (đạt) / warn (cần cải thiện) / fail (không đạt).

| # | Nhóm | Tiêu chí | Ngưỡng đạt |
|---|------|----------|------------|
| 1 | Tiêu đề H1 | Độ dài H1 | < 65 ký tự |
| 2 | Tiêu đề H1 | Chứa từ khóa chính | Có mặt trong H1 |
| 3 | Tiêu đề SEO | Độ dài title tag | < 65 ký tự |
| 4 | Tiêu đề SEO | Có CTR signal | Có số liệu/năm/câu hỏi/power word |
| 5 | Mô tả SEO | Độ dài meta description | 120–160 ký tự |
| 6 | Mô tả SEO | Chứa từ khóa | Có mặt trong meta description |
| 7 | Sapo đầu bài | Từ khóa trong 150 ký tự đầu | Có mặt |
| 8 | Sapo đầu bài | Bôi đậm từ khóa | Có thẻ `<b>`/`<strong>` quanh từ khóa |
| 9 | Trình bày | Có bold/highlight trong bài | ≥ 1 đoạn có bold |
| 10 | Trình bày | Độ dài câu | Câu trung bình < 25 từ |
| 11 | Trình bày | Độ dài đoạn | Đoạn trung bình < 4 câu |
| 12 | UL–LI | Có bullet/numbered list | ≥ 1 list trong bài |
| 13 | Heading | Có H2 | ≥ 2 heading H2 |
| 14 | Heading | Có H3 khi cần | H3 xuất hiện dưới H2 dài |
| 15 | Heading | Từ khóa trong heading | ≥ 1 H2 chứa từ khóa hoặc biến thể |
| 16 | Mật độ từ khóa | Keyword density | 1–3% tổng số từ |
| 17 | Tối ưu ảnh | ALT text | Mọi ảnh có ALT |
| 18 | Tối ưu ảnh | Từ khóa trong ALT | ≥ 1 ảnh có ALT chứa từ khóa |
| 19 | Liên kết | Internal + external link | ≥ 2 internal link, ≥ 1 external link uy tín |
| 20 | Đoạn kết bài | Từ khóa trong 150 ký tự cuối | Có mặt |

## Cách tính điểm tổng
- Mỗi tiêu chí pass = 5 điểm, warn = 2.5 điểm, fail = 0 điểm
- Tổng điểm = (tổng điểm đạt được / (số tiêu chí × 5)) × 100
- Dưới 50/100: cần viết lại; 50–75: cần chỉnh sửa; trên 75: đạt chuẩn SEONGON

## Nguồn
Dựa trên checklist thực tế của SEONGON, đồng bộ với logic tại `backend/analyzer.py` trong repo `seo-optimizer`.
