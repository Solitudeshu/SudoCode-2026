# Text Preprocessing

Bài tập Tuần 1 của chương trình **Sudo Code 2026**.

> **Yêu cầu:** Tiền xử lý một tập dữ liệu văn bản cho trước, báo cáo các bước thực hiện và kết quả.

## Dataset

**Vietnamese Online News Dataset**

Dataset gồm các bài báo tiếng Việt với các trường như `title`, `content`, `author`, `source`, `topic`, `url`,...

* Số lượng ban đầu: **184,539 bài báo**
* Trường văn bản chính được sử dụng để tiền xử lý: `content`
* Nguồn: Vietnamese Online News Dataset trên Kaggle

## Quy trình tiền xử lý

Notebook thực hiện các bước chính:

1. Đọc và khảo sát dữ liệu.
2. Loại bỏ các bài viết bị thiếu hoặc rỗng.
3. Loại bỏ các bài viết trùng lặp.
4. Chuẩn hóa Unicode tiếng Việt theo NFC.
5. Loại bỏ HTML, URL, email, dấu câu và các ký tự đặc biệt.
6. Chuyển văn bản về chữ thường và chuẩn hóa khoảng trắng.
7. Tách từ tiếng Việt bằng `underthesea`.
8. Loại bỏ một số stopword tiếng Việt.
9. So sánh dữ liệu trước và sau tiền xử lý.
10. Phân tích các từ xuất hiện thường xuyên.
11. Xuất dataset sau khi đã tiền xử lý.

Trong quá trình xử lý, **dấu tiếng Việt và thông tin số được giữ lại**.

## Kết quả

| Chỉ số                             | Kết quả |
| ---------------------------------- | ------: |
| Số văn bản ban đầu                 | 184,539 |
| Văn bản thiếu/rỗng bị loại         |  23,468 |
| Văn bản trùng lặp bị loại          |   4,519 |
| Số văn bản còn lại                 | 156,552 |
| Tỷ lệ dữ liệu được giữ lại         |  84.83% |
| Số từ trung bình trước tiền xử lý  |  595.94 |
| Số token trung bình sau tiền xử lý |  365.07 |

Dataset sau xử lý gồm ba dạng biểu diễn văn bản chính:

* `clean_text`: văn bản đã được làm sạch và chuẩn hóa.
* `tokenized_text`: văn bản sau khi tách từ tiếng Việt.
* `processed_text`: văn bản sau khi tách từ và loại bỏ stopword.

## Công cụ sử dụng

* Python
* pandas
* Matplotlib
* underthesea
* Regular Expressions (`re`)

## Notebook

[`32_TextPreprocessing.ipynb`](./32_TextPreprocessing.ipynb)

