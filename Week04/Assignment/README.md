# Sentiment Analysis with SVM and Naive Bayes

Bài tập Tuần 4 của chương trình **Sudo Code 2026**.

> **Yêu cầu:** Perform sentiment analysis using SVM and Naive Bayes and compare the results.

## Datasets

| Dataset | Dữ liệu ban đầu được đọc | Train sau xử lý | Test | Đặc trưng TF-IDF |
|---|---:|---:|---:|---:|
| Vietnamese Product Reviews | 3,040 | 2,392 | 599 | 8,882 |
| UIT-VSFC | 11,426 train + 3,166 test | 11,293 | 3,166 | 12,996 |

- **[Vietnamese Product Reviews](https://www.kaggle.com/datasets/tuannguyenvananh/vietnamese-text-classification-dataset):** đọc toàn bộ `train.csv` không có header; loại 49 mẫu, chia 80% train / 20% test có stratify, seed 42. Giữ nhãn **0, 1, 2** khi báo cáo vì Data Card chưa công bố định nghĩa nhãn.
- **[UIT-VSFC](https://sites.google.com/uit.edu.vn/uit-nlp/datasets):** phản hồi sinh viên, được chọn từ [danh sách GitHub của đề](https://github.com/undertheseanlp/NLP-Vietnamese-progress/blob/master/tasks/sentiment_analysis.md). Dùng nhãn sentiment: **0 = negative, 1 = neutral, 2 = positive**. Giữ cách chia train/test gốc, không dùng dev vì không tuning. Loại 133 mẫu train, trong đó 46 mẫu trùng văn bản test; giữ toàn bộ test. Train đã khác bản benchmark gốc.

## Method

1. Loại dữ liệu thiếu/rỗng, xử lý mẫu trùng và nhãn mâu thuẫn.
2. Chuẩn hóa Unicode NFC, chữ thường, làm sạch HTML/URL/dấu câu và tách từ bằng `underthesea`; giữ dấu tiếng Việt, stopword và từ phủ định.
3. Dùng **TF-IDF unigram + bigram**, `min_df=2`, `sublinear_tf=True`, tối đa 20,000 đặc trưng. Chỉ fit trên train, giữ ma trận sparse.
4. Huấn luyện **SVM (`LinearSVC`, C=1)** và **Naive Bayes (`MultinomialNB`, alpha=1)** trên cùng dữ liệu, với tham số cố định.
5. So sánh Accuracy, Precision/Recall/Macro-F1, thời gian huấn luyện và dự đoán; báo cáo từng lớp và confusion matrix trong notebook.

## Results

Kết quả chạy trên **Kaggle CPU**, scikit-learn **1.6.1**, underthesea **8.3.0**:

| Dataset | Model | Accuracy | Macro-F1 | Train (s) |
|---|---|---:|---:|---:|
| Product Reviews | SVM | **0.8097** | **0.7987** | 0.0393 |
| Product Reviews | Naive Bayes | 0.7579 | 0.7214 | **0.0054** |
| UIT-VSFC | SVM | **0.8948** | **0.6981** | 0.1017 |
| UIT-VSFC | Naive Bayes | 0.8699 | 0.5956 | **0.0055** |

- **Product Reviews:** SVM cao hơn **7.73 điểm phần trăm Macro-F1**. Lớp 1 khó nhất: F1 của SVM đạt **0.6748**, Naive Bayes đạt **0.5077**.
- **UIT-VSFC:** SVM cao hơn **10.26 điểm phần trăm Macro-F1**. Lớp neutral chỉ có **167 mẫu test (5.27%)**; SVM nhận đúng **28 mẫu**, Naive Bayes không dự đoán mẫu nào là neutral, nên F1 lớp này bằng **0**. Accuracy cao chưa phản ánh đầy đủ chất lượng trên cả ba lớp.
- **Naive Bayes huấn luyện nhanh hơn** trong cả hai thí nghiệm. Thời gian toàn notebook theo log Kaggle: **77.3 giây** cho Product Reviews và **53.3 giây** cho UIT-VSFC. Cột Train chỉ đo bộ phân loại, không gồm tiền xử lý và TF-IDF.

Với cấu hình hiện tại, **SVM tốt hơn theo Macro-F1 trên cả hai dataset**. Kết luận giới hạn ở từng tập test.

Bảng số liệu đầy đủ: [Product Reviews CSV](./outputs/kaggle_reviews_results.csv) · [UIT-VSFC CSV](./outputs/uit_vsfc_results.csv).

## Tools

Python, pandas, scikit-learn, underthesea và Matplotlib.

## Notebooks

| Dataset | Notebook trong repository | Kaggle |
|---|---|---|
| Product Reviews | [32_SentimentAnalysis_Kaggle.ipynb](./32_SentimentAnalysis_Kaggle.ipynb) | [32-sentimentanalysis](https://www.kaggle.com/code/solitudeshu/32-sentimentanalysis) |
| UIT-VSFC | [32_SentimentAnalysis_UITVSFC.ipynb](./32_SentimentAnalysis_UITVSFC.ipynb) | [32-sentimentanalysis-uitvsfc](https://www.kaggle.com/code/solitudeshu/32-sentimentanalysis-uitvsfc) |

## References

- [scikit-learn — Supervised Learning](https://scikit-learn.org/stable/supervised_learning.html#supervised-learning), [SVM](https://scikit-learn.org/stable/modules/svm.html), [Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html).
- Nguyen et al., *UIT-VSFC: Vietnamese Students’ Feedback Corpus for Sentiment Analysis*, KSE 2018. [Dữ liệu và README gốc](https://drive.google.com/drive/folders/1xclbjHHK58zk2X6iqbvMPS2rcy9y9E0X).
