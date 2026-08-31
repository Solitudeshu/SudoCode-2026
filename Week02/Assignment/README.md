# Text Feature Extraction

Bài tập Tuần 2 của chương trình **Sudo Code 2026**.

> **Yêu cầu:** Áp dụng 4 kỹ thuật trên một tập dữ liệu văn bản cho trước.

## Dataset

**Vietnamese Online News Dataset**

Dataset gồm các bài báo tiếng Việt với các trường như `title`, `content`, `author`, `source`, `topic`, `url`,...

* Số lượng ban đầu: **184,539 bài báo**
* Trường văn bản chính được sử dụng để trích xuất đặc trưng: `content`
* Nguồn: Vietnamese Online News Dataset trên Kaggle

## Quy trình thực hiện

Notebook thực hiện các bước chính:

1. Đọc dữ liệu và lựa chọn trường `content`.
2. Loại bỏ các bài viết bị thiếu, rỗng hoặc trùng lặp.
3. Làm sạch, chuẩn hóa và tách từ tiếng Việt theo quy trình của Tuần 1.
4. Loại bỏ một số stopword tiếng Việt.
5. Biểu diễn văn bản bằng **Bag of Words**.
6. Biểu diễn văn bản bằng **Word N-grams** gồm unigram và bigram.
7. Tính trọng số đặc trưng bằng **TF-IDF**.
8. Biểu diễn văn bản bằng **Feature Hashing**.
9. So sánh kích thước, số phần tử khác 0 và mật độ của các ma trận đặc trưng.
10. Trực quan hóa các đặc trưng có tổng trọng số cao nhất.

Các ma trận đặc trưng được giữ ở dạng **sparse matrix** để tiết kiệm bộ nhớ. Với Bag of Words, Word N-grams và TF-IDF, số đặc trưng tối đa được giới hạn ở **20,000**.

## Bốn kỹ thuật trích xuất đặc trưng

* **Bag of Words:** đếm số lần xuất hiện của từng unigram trong mỗi tài liệu.
* **Word N-grams:** bổ sung các cặp từ liên tiếp để giữ một phần thông tin ngữ cảnh cục bộ.
* **TF-IDF:** giảm trọng số của các từ phổ biến và làm nổi bật các từ có tính phân biệt.
* **Feature Hashing:** ánh xạ token vào không gian có số chiều cố định mà không cần lưu vocabulary.

## Kết quả

Sau bước chuẩn bị, **156,548 văn bản** được sử dụng để trích xuất đặc trưng.

| Kỹ thuật        | Số văn bản | Số đặc trưng | Số phần tử khác 0 | Mật độ (%) |
| --------------- | ----------: | -----------: | -----------------: | ---------: |
| Bag of Words    |     156,548 |       20,000 |         29,939,920 |     0.9563 |
| Word N-grams    |     156,548 |       20,000 |         39,754,505 |     1.2697 |
| TF-IDF          |     156,548 |       20,000 |         39,754,505 |     1.2697 |
| Feature Hashing |     156,548 |       65,536 |         31,630,508 |     0.3083 |

Kết quả cho thấy các biểu diễn văn bản đều tạo ra ma trận rất thưa. Word N-grams giữ thêm thông tin về các cặp từ nên có nhiều phần tử khác 0 hơn Bag of Words. TF-IDF có cùng cấu trúc khác 0 với Word N-grams nhưng thay số đếm bằng trọng số. Feature Hashing sử dụng không gian cố định 65,536 chiều và không cung cấp tên đặc trưng.

## Công cụ sử dụng

* Python
* pandas
* NumPy
* Matplotlib
* scikit-learn
* underthesea
* Regular Expressions (`re`)

## Notebook

[`32_TextFeatureExtraction.ipynb`](./32_TextFeatureExtraction.ipynb)
