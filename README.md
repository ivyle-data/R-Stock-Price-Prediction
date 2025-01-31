# Dự đoán Giá Cổ phiếu bằng Mô hình Học sâu RNN và LSTM

## Giới thiệu
Dự án này tập trung vào việc phân tích và dự đoán giá cổ phiếu của ba ngân hàng Việt Nam (VCB, ACB, BIDV) sử dụng mô hình học sâu RNN (Recurrent Neural Network) và LSTM (Long Short-Term Memory). Báo cáo đánh giá hiệu suất của các mô hình dựa trên các chỉ số MSE, RMSE, MAPE và R-Squared.

---

## Cấu trúc thư mục
```plaintext
Stock_Price_Prediction
│── README.md              # Giới thiệu dự án
│── reports/               # Báo cáo phân tích
│── data/                  # Chứa dữ liệu đầu vào
│── scripts/               # Chứa mã R Markdown
```

## Cách sử dụng
### 1. Cài đặt môi trường
```r
install.packages(c("tidyverse", "ggplot2", "forecast", "tensorflow", "keras"))
```

### 2. Chạy mã R Markdown
Chạy tệp `Stock_Prediction_Analysis.rmd` bằng cách mở trong RStudio và chạy từng bước.
Hoặc sử dụng dòng lệnh R để tạo báo cáo:
```r
rmarkdown::render("Stock_Prediction_Analysis.rmd")
```

## Kết quả
Kết quả dự báo giá được lưu trong thư mục `reports/`.

## Tổng quan

### Bối cảnh
Phân tích dữ liệu chứng khoán giúp nắm bắt xu hướng thị trường và dự đoán giá cổ phiếu. Các phương pháp truyền thống (ARIMA, ML) có hạn chế trong xử lý chuỗi thời gian phức tạp. Học sâu (Deep Learning) với RNN và LSTM được ứng dụng để cải thiện độ chính xác.

### Mục tiêu
- So sánh hiệu suất của RNN và LSTM trong dự đoán giá cổ phiếu.
- Xác định tham số tối ưu (window size, tỉ lệ dữ liệu huấn luyện/kiểm tra).

### Dữ liệu
- **Nguồn dữ liệu**: Giá cổ phiếu 3 ngân hàng (VCB: 2013–2023, ACB: 2013–2023, BIDV: 2014–2023).
- **Biến chính**: Giá mở cửa, cao nhất, thấp nhất, đóng cửa, khối lượng giao dịch.

## Phương pháp

### Tiền xử lý dữ liệu
- Chuẩn hóa dữ liệu bằng **Min-Max Scaling**.
- Xử lý dữ liệu thiếu bằng **Nội suy tuyến tính**.
- Phân tích thành phần xu hướng (trend) và thời vụ (seasonality).

### Mô hình
1. **RNN**:
   - Kiến trúc: 4 lớp ẩn (64, 128, 256, 512 units), Dropout 0.2.
   - Huấn luyện: 40 epochs, Batch size 32, Optimizer Adam.

2. **LSTM**:
   - Kiến trúc: 3 lớp LSTM (10 units), Learning rate 0.01.
   - Huấn luyện: 40 epochs, Batch size 4, Optimizer Adam.

### Chỉ số đánh giá
- **MSE (Mean Squared Error)**
- **RMSE (Root Mean Squared Error)**
- **MAPE (Mean Absolute Percentage Error)**
- **R-Squared**

## Kết quả chính

### Hiệu suất mô hình RNN
| Ngân hàng | Window Size | MSE       | RMSE     | MAPE (%) | R-Squared |
|-----------|-------------|-----------|----------|----------|-----------|
| VCB       | 1           | 0.000399  | 0.01997  | 1.77     | 0.882     |
| ACB       | 1           | 0.00014   | 0.01183  | 2.65     | 0.898     |
| BIDV      | 2           | 0.000261  | 0.01614  | 1.42     | 0.763     |

### Hiệu suất mô hình LSTM
| Ngân hàng | Window Size | MSE       | RMSE     | MAPE (%) | R-Squared |
|-----------|-------------|-----------|----------|----------|-----------|
| VCB       | 1           | 0.000808  | 0.02842  | 1.94     | 0.858     |
| ACB       | 6           | 0.000158  | 0.01257  | 3.27     | 0.885     |
| BIDV      | 6           | 0.000236  | 0.01535  | 1.46     | 0.786     |

### Nhận xét
- **RNN** cho kết quả tốt hơn trên cả 3 ngân hàng, đặc biệt với window size nhỏ (1-2).
- **LSTM** phù hợp hơn với dữ liệu có tính thời vụ phức tạp (ACB window size 6).

## Kết luận & Khuyến nghị

### Kết luận
- Cả RNN và LSTM đều hiệu quả trong dự đoán giá cổ phiếu, nhưng RNN phù hợp hơn với dữ liệu nhỏ.
- Dữ liệu giá đóng cửa kết hợp khối lượng giao dịch là yếu tố quan trọng.
Mô hình RNN hoạt động tốt hơn trên cả ba dữ liệu ngân hàng, mặc dù mô hình LSTM được đánh giá cao hơn về mặt lý thuyết. Điều này có thể là do dữ liệu của cả ba ngân hàng còn khá ít (trung bình 2.650 dòng/ngân hàng), đủ để mô hình RNN hoạt động tốt nhưng chưa đủ để mô hình LSTM phát huy hiệu quả.

### Hạn chế
- Mô hình chưa xử lý tốt nhiễu dữ liệu và biến động thị trường đột ngột.
- Thời gian huấn luyện dài do độ phức tạp của LSTM.

### Khuyến nghị
- **Mở rộng dữ liệu**: Kết hợp tin tức, chỉ số kinh tế vĩ mô.
- **Tối ưu mô hình**: Sử dụng Attention Mechanism, Grid Search cho hyperparameter.
- **Ứng dụng thực tế**: Xây dựng hệ thống cảnh báo xu hướng cho nhà đầu tư.

---

**Tác giả**: Lê Huỳnh Thúy Vy
