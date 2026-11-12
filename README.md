# Vietnamese Toxic Text Classification

> **Lưu ý:** Dự án này được tiếp tục phát triển từ repository : [Link Repo Gốc](https://github.com/hugo-tng/NLP_Final_Project/tree/main/Vietnamese_text_sumarization).

---

## 📌 Giới thiệu

Dự án nghiên cứu và xây dựng mô hình phân loại nhận xét/bình luận độc hại (Toxic Text Classification) trong tiếng Việt, sử dụng mô hình ngôn ngữ tiền huấn luyện **PhoBERT** và kỹ thuật sinh dữ liệu tổng hợp (**Synthetic Data**) qua Gemini API.

Hai nhãn phân loại chính:

- `toxic`: Bình luận có chứa ngôn từ độc hại, lăng mạ, xúc phạm, công kích.
- `non-toxic`: Bình luận bình thường, lành mạnh.

---

## 📁 Cấu trúc thư mục

```text
Vietnamese-toxic-text-classification/
├── Datasets/
│   ├── Dataset01.csv                   # Dữ liệu gốc đã gán nhãn (~6.7k dòng)
│   └── synthetic_final.csv             # Dữ liệu tổng hợp bổ sung (~19.4k dòng)
├── Source/
│   ├── Encoder_Only_Finetune.ipynb     # Notebook fine-tuning mô hình PhoBERT
│   └── Synthetic_Data_EncodeOnly.ipynb # Notebook sinh dữ liệu với Gemini API
├── .gitignore
└── README.md
```

---

## 📊 Dữ liệu

Dự án sử dụng 2 tệp dữ liệu dạng CSV trong thư mục `Datasets/`:

- **`Dataset01.csv`**: Dữ liệu bình luận tiếng Việt ban đầu, gồm các cột:
  - `id`: Mã định danh bình luận.
  - `text`: Nội dung văn bản bình luận.
  - `label`: Nhãn phân loại (`toxic` / `non-toxic`).
- **`synthetic_final.csv`**: Tập dữ liệu đã được gộp từ dữ liệu gốc và dữ liệu tổng hợp sinh thêm nhằm tăng độ đa dạng cho tập huấn luyện.

---

## 🛠️ Quy trình thực hiện

1. **Sinh dữ liệu tổng hợp (`Source/Synthetic_Data_EncodeOnly.ipynb`):**
   - Sử dụng thư viện `google-genai` và Gemini API để sinh các biến thể văn bản tiếng Việt.
   - Làm sạch, kiểm tra và gộp dữ liệu thành tệp `synthetic_final.csv`.

2. **Huấn luyện mô hình (`Source/Encoder_Only_Finetune.ipynb`):**
   - Tải và tiền xử lý dữ liệu.
   - Chia tập dữ liệu (Train / Validation / Test).
   - Tokenize văn bản với `AutoTokenizer` của PhoBERT (`vinai/phobert-large` hoặc `vinai/phobert-base`).
   - Fine-tune mô hình phân loại tuần tự với Hugging Face `Trainer`.
   - Đánh giá hiệu quả mô hình qua các chỉ số: Accuracy, Precision, Recall, F1-Score.

---

## 🚀 Hướng dẫn cài đặt & Chạy

### 1. Cài đặt thư viện

Cài đặt các gói phụ thuộc cần thiết:

```bash
pip install torch transformers datasets evaluate scikit-learn pandas numpy google-genai underthesea
```

### 2. Chạy Notebooks

Khởi động Jupyter Notebook hoặc mở trên Google Colab / VS Code:

- Chạy `Source/Synthetic_Data_EncodeOnly.ipynb` nếu cần sinh thêm dữ liệu (yêu cầu cấu hình `GEMINI_API_KEY`).
- Chạy `Source/Encoder_Only_Finetune.ipynb` để huấn luyện và đánh giá mô hình.

---

## ⚠️ Tuyên bố miễn trừ trách nhiệm (Disclaimer)

- **Mục đích học thuật:** Dự án được thực hiện nhằm phục vụ mục đích **học tập, nghiên cứu và thử nghiệm học thuật** trong khuôn khổ môn học Xử lý ngôn ngữ tự nhiên (NLP).
- **Nội dung dữ liệu:** Tập dữ liệu có chứa các ngôn từ thô tục, xúc phạm hoặc nhạy cảm do đặc thù của bài toán phân loại văn bản độc hại; các nội dung này hoàn toàn không phản ánh quan điểm hay lập trường của tác giả.
- **Giới hạn sử dụng:** Kết quả và mô hình chỉ mang tính chất nghiên cứu, thử nghiệm và tham khảo; không được sử dụng cho mục đích thương mại hay làm căn cứ pháp lý, kiểm duyệt thực tế.
