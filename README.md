# DỰ BÁO NỒNG ĐỘ BỤI MỊN PM2.5 TẠI HÀ NỘI TRONG 3 THÁNG ĐẦU NĂM 2026
**Môn học:** MAT3388 - PHÂN TÍCH CHUỖI THỜI GIAN

Dự án phân tích và dự báo chuỗi thời gian nồng độ bụi mịn PM2.5 tại Hà Nội. Dự án thực hiện đánh giá trên ba độ phân giải thời gian khác nhau (theo giờ, 6 giờ, và theo ngày) bằng cách sử dụng và so sánh các mô hình thống kê truyền thống (ARIMA/SARIMA) cùng với mô hình mở rộng ARIMAX với các mô hình học máy/học sâu (XGBoost, LSTM), kết hợp kỹ thuật phân rã chuỗi thời gian MSTL.

## Nội dung
- [Tính năng chính](#tính-năng-chính)
- [Công nghệ](#công-nghệ)
- [Cài đặt](#cài-đặt)
- [Dữ liệu đầu vào](#dữ-liệu-đầu-vào)
- [Cấu trúc thư mục](#cấu-trúc-thư-mục)
- [Phương pháp (tóm tắt)](#phương-pháp-tóm-tắt)

## Tính năng chính
| Hạng mục | Mô tả |
| --- | --- |
| **EDA & Tiền xử lý** | Làm sạch dữ liệu, xử lý giá trị thiếu, thống kê mô tả nồng độ PM2.5. Phân rã chuỗi thời gian bằng MSTL để quan sát xu hướng và mùa vụ. |
| **Dự báo đa khung thời gian** | Xây dựng pipeline phân tích ở 3 cấp độ: theo giờ (hourly), mỗi 6 giờ (6-hour) và theo ngày (daily). |
| **Mô hình Thống kê** | Ứng dụng `auto_arima` để tìm kiếm tham số tối ưu cho mô hình ARIMA/SARIMA, đánh giá kiểm định tính dừng (ADF). |
| **Mô hình Machine Learning / Deep Learning** | Áp dụng XGBoost và mạng nơ-ron LSTM với các đặc trưng lag, rolling window và các biến trích xuất từ dữ liệu thời gian. |
| **Đánh giá & So sánh** | So sánh hiệu năng các mô hình qua RMSE, MAE, MAPE và trực quan hóa kết quả thực tế vs dự báo. |

## Công nghệ
- **Python 3.8+**
- `pandas`, `numpy` — xử lý dữ liệu.
- `pmdarima`, `statsmodels` — SARIMA / ARIMA / MSTL / kiểm định chuỗi thời gian.
- `xgboost` — mô hình học máy nâng cao.
- `tensorflow.keras` — mô hình học sâu LSTM cho chuỗi thời gian.
- `matplotlib`, `seaborn` — trực quan hóa biểu đồ.
- `scikit-learn` — tính toán độ lỗi (metrics), thiết kế pipeline huấn luyện.

## Cài đặt
```bash
git clone <repository-url>
cd Hanoi-PM2.5-prediction-project

# Tạo và kích hoạt môi trường ảo (ví dụ trên Windows)
python -m venv .venv
.venv\Scripts\activate

# Cài đặt thư viện cần thiết
pip install -r requirements.txt
```

## Dữ liệu đầu vào
- **Thời gian dự báo:** 3 tháng đầu năm 2026 (Quý 1).
- **Vị trí:** Hà Nội.
- Dữ liệu thô và đã qua xử lý được lưu trong thư mục `data/`. Bao gồm các tập train/test theo ngày, theo 6 giờ và theo giờ.

## Cấu trúc thư mục
```text
Hanoi-PM2.5-prediction-project/
├── data/
│   ├── PM25_2026_Q1_raw.csv           # Dữ liệu gốc
│   ├── PM25_2026_Q1_train.csv         # Tập huấn luyện (1 giờ)
│   ├── PM25_2026_Q1_test_1week.csv    # Tập kiểm thử (1 giờ)
│   ├── PM25_2026_Q1_train_6h.csv      # Tập huấn luyện (6 giờ)
│   ├── PM25_2026_Q1_test_6h.csv       # Tập kiểm thử (6 giờ)
│   ├── PM25_2026_Q1_train_daily.csv   # Tập huấn luyện (ngày)
│   └── PM25_2026_Q1_test_daily.csv    # Tập kiểm thử (ngày)
│
├── notebooks/
│   ├── 01_EDA.ipynb                   # Bước 1: Khám phá và tiền xử lý dữ liệu
│   ├── 02_hour_forecasting.ipynb      # Bước 2: Dự báo theo giờ (ARIMA, XGBoost, LSTM)
│   ├── 03_six-hour_forecasting.ipynb  # Bước 3: Dự báo mỗi 6 giờ
│   └── 04_daily_forecasting.ipynb     # Bước 4: Dự báo theo ngày
│
├── reports/
│   ├── Time_Series_PM25_Forecasting_Report.pdf  # Báo cáo đồ án chi tiết
│   └── Time_Series_PM25_Forecasting_Slide.pdf   # Slide thuyết trình
│
└── README.md                          # Tài liệu mô tả dự án
```

## Phương pháp (tóm tắt)
1. **EDA:** Làm sạch, nội suy các khoảng khuyết dữ liệu. Trực quan hóa tương quan phân rã chuỗi theo MSTL.
2. **Resample dữ liệu:** Chuẩn bị sẵn dữ liệu dưới 3 góc độ: 1-giờ, 6-giờ, 1-ngày để so sánh khả năng dự báo của từng khung thời gian.
3. **Mô hình Thống kê (ARIMA/SARIMA):** Sử dụng `auto_arima` kết hợp grid search tự động để tìm kiếm (p,d,q) tối ưu dựa trên AIC/BIC. Chẩn đoán phần dư kỹ lưỡng.
4. **Mô hình ML/DL:** Xây dựng mô hình với tập đặc trưng mở rộng (lags, phân rã MSTL) thông qua XGBoost và LSTM, điều chỉnh batch_size/epochs và cấu trúc lớp mạng cho phù hợp.
5. **Đánh giá kết quả:** Dự báo trên tập kiểm thử (test set). Kết quả đánh giá bằng các metric (RMSE, MAE, MAPE). Các biểu đồ "Actual vs Predicted" được xuất trực tiếp trong các notebook để đánh giá độ khớp.

## Tài liệu tham khảo trong repo
- [`reports/Time_Series_PM25_Forecasting_Report.pdf`](reports/Time_Series_PM25_Forecasting_Report.pdf) — Báo cáo đồ án chi tiết về phương pháp và kết quả.
- [`reports/Time_Series_PM25_Forecasting_Slide.pdf`](reports/Time_Series_PM25_Forecasting_Slide.pdf) — Slide thuyết trình đồ án cuối kỳ.
