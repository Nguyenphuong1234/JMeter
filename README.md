# BÁO CÁO THỰC HÀNH KIỂM THỬ HIỆU NĂNG BẰNG APACHE JMETER

## 1. Thông tin chung & Môi trường thử nghiệm
- **Công cụ kiểm thử**: Apache JMeter (Phiên bản 5.6.3)
- **Mục tiêu**: Mô phỏng tải người dùng truy cập API để đánh giá khả năng xử lý, thời gian phản hồi và độ ổn định của máy chủ.
- **Đối tượng kiểm thử (Target Server)**:
  - **Protocol**: `https`
  - **Server Name**: `reqres.in`
  - **HTTP Method**: `GET`
  - **Path**: `/api/users?page=2`

---

## 2. Kịch bản kiểm thử (Test Plan Scenario)
Cấu hình nhóm người dùng (**Thread Group**) giả lập tải như sau:
- **Number of Threads (Users)**: `5` (Mô phỏng 5 người dùng đồng thời).
- **Ramp-up period (seconds)**: `5` (Tất cả 5 users sẽ khởi tạo lần lượt trong vòng 5 giây).
- **Loop Count**: `2` (Mỗi user thực hiện lại kịch bản 2 lần).
- **Tổng số lượng Yêu cầu (Total Requests)**: 5 users x 2 loops = 10 requests.

---

## 3. Các bước triển khai trên JMeter
1. **Tạo Thread Group**: Nhấp chuột phải vào `Test Plan` -> `Add` -> `Threads (Users)` -> `Thread Group` và thiết lập các thông số về tải.
2. **Cấu hình Sampler**: Nhấp chuột phải vào `Thread Group` -> `Add` -> `Sampler` -> `HTTP Request` để điền địa chỉ Server, phương thức `GET` và đường dẫn API.
3. **Thêm các Bộ lắng nghe (Listeners)**:
   - `View Results Tree`: Theo dõi tình trạng phản hồi chi tiết của từng yêu cầu.
   - `Summary Report`: Thống kê đo lường các chỉ số hiệu năng tổng hợp (Response Time, Error %, Throughput).
4. **Thực thi và lưu kết quả**: Bấm `Run` để gửi tải và xuất các chỉ số đo lường.

---

## 4. Báo cáo Kết quả Chi tiết (Detailed Results)

### a) Cấu hình kịch bản gửi HTTP Request
Gửi request phương thức `GET` tới đường dẫn `/api/users?page=2` thông qua giao thức HTTPS.

<img width="1920" height="1024" alt="Hình 1" src="https://github.com/user-attachments/assets/c89f4865-0483-49eb-bdf3-97429f29e6c0" />


---

### b) Chi tiết thực thi theo từng Request (View Results Tree)
Toàn bộ 10/10 requests đều hiển thị trạng thái màu xanh (thành công).
- **HTTP Response Code**: `200 OK` (Máy chủ tiếp nhận và trả về dữ liệu thành công).
- Không phát sinh lỗi Timeout hay lỗi kết nối từ phía Client/Server.

<img width="1918" height="1035" alt="Hinh 2" src="https://github.com/user-attachments/assets/4db8d782-e1ef-4d23-a259-0c00e6114a5f" />


---

### c) Bảng thống kê hiệu năng chi tiết (Summary Report)
Dữ liệu trích xuất trực tiếp từ bảng kết quả chạy thực tế:

| Chỉ số (Metric) | Giá trị đo được | Giải thích ý nghĩa |
| :--- | :--- | :--- |
| **# Samples** | `10` | Tổng số lượng request đã gửi đến máy chủ. |
| **Average Response Time** | `3294 ms` (~3.29 giây) | Thời gian phản hồi trung bình của một request. |
| **Min Response Time** | `28 ms` | Thời gian phản hồi nhanh nhất thu được. |
| **Max Response Time** | `8381 ms` (~8.38 giây) | Thời gian phản hồi chậm nhất thu được. |
| **Std. Dev. (Độ lệch chuẩn)**| `3401.56` | Độ chênh lệch thời gian phản hồi giữa các request. |
| **Error %** | **`0.00%`** | Tỷ lệ lỗi phát sinh (Tất cả request đều thành công 100%). |
| **Throughput (Năng suất)** | `1.1 / sec` | Số lượng request máy chủ xử lý được trung bình trong 1 giây. |
| **Received KB/sec** | `3.93 KB/s` | Băng thông dữ liệu tải về trung bình mỗi giây. |
| **Sent KB/sec** | `0.14 KB/s` | Băng thông dữ liệu gửi đi trung bình mỗi giây. |
| **Avg. Bytes** | `3504.0 Bytes` | Kích thước dữ liệu trung bình thu được của một response. |

<img width="1916" height="1031" alt="Hinh 3" src="https://github.com/user-attachments/assets/115396b1-fbf0-4505-96ca-65e6f28118ff" />


---

## 5. Phân tích & Đánh giá (Conclusion & Evaluation)
1. **Độ tin cậy (Reliability)**: Kịch bản thử nghiệm đạt tỷ lệ thành công tuyệt đối **100% (Error % = 0.00%)** dưới mức tải 5 users đồng thời.
2. **Thời gian phản hồi (Response Time)**: Thời gian phản hồi trung bình đạt **3294 ms**.
3. **Kết luận chung**: Máy chủ đáp ứng tốt kịch bản kiểm thử tải cơ bản, không có request nào bị treo hoặc trả về lỗi HTTP 4xx/5xx.
