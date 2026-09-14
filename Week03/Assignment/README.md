# Word2Vec

Bài tập Tuần 3 của chương trình **Sudo Code 2026**.

> **Yêu cầu:** Implement Word2Vec on a text dataset and report the embeddings visualization.

## Dataset

**[viwik18](https://github.com/NTT123/viwik18)** — Wikipedia tiếng Việt tháng 08/2018 đã làm sạch.

* Sử dụng toàn bộ **10 file**, tổng **874.4 MiB**, không lấy mẫu corpus.
* Dữ liệu được chia thành khối **200 âm tiết** để xử lý; đây không phải câu tự nhiên vì corpus đã mất dấu câu.

## Quy trình thực hiện

1. Ghép các file nhị phân theo thứ tự để bảo toàn UTF-8.
2. Chuẩn hóa Unicode NFC, tách từ tiếng Việt bằng `underthesea` và giữ stopword.
3. Huấn luyện Word2Vec **Skip-gram + Negative Sampling** từ đầu.
4. Kiểm tra các từ gần nhau bằng cosine similarity.
5. Trực quan hóa **36 từ** bằng t-SNE và nhận xét kết quả trong notebook.

Cấu hình: **100 chiều**, window **5**, min_count **5**, negative **5**, sample **0.001**, **10 epochs**. Xử lý theo batch và đọc corpus từ đĩa để tiết kiệm RAM.

## Kết quả

Kết quả chạy trên Kaggle bằng **CPU, 4 workers**:

| Chỉ số | Kết quả |
|---|---:|
| Số khối văn bản | 751,503 |
| Token sau tách từ | 120,226,378 |
| Vocabulary sau min_count | 418,412 từ |
| Kích thước ma trận embeddings | 418,412 × 100 |
| Thời gian tiền xử lý | 72.8 phút |
| Xây vocabulary và huấn luyện | 56.7 phút |
| Tổng thời gian notebook | 2 giờ 11 phút |

Các cặp có quan hệ phù hợp gồm `bóng_đá`–`cầu_thủ` (**0.8667**), `máy_tính`–`máy_vi_tính` (**0.8829**) và `đại_học`–`trường` (**0.8908**). Biểu đồ thể hiện bốn nhóm truy vấn khá rõ; một số token như `quốc_cô`, `đại_hoc` vẫn cho thấy nhiễu dữ liệu/tách từ.

t-SNE chỉ minh họa các từ được chọn, không đánh giá toàn vocabulary. Notebook ghi rõ chênh lệch khoảng **0.051%** giữa số token đầu vào và số raw words được log mỗi epoch.

## Công cụ sử dụng

Python, Gensim, underthesea, joblib, NumPy, pandas, scikit-learn, Matplotlib và adjustText.

## Notebook

* [32_Word2Vec.ipynb](./32_Word2Vec.ipynb)
* [Notebook trên Kaggle](https://www.kaggle.com/code/solitudeshu/32-word2vec)
