# BÁO CÁO THỰC HÀNH KIỂM THỬ HIỆU NĂNG BẰNG APACHE JMETER

---

## I. Môi trường & Kịch bản kiểm thử (Test Scenario)

### 1. Mục tiêu kiểm thử
- **Đánh giá năng lực xử lý**: Kiểm tra khả năng chịu tải và độ ổn định của máy chủ Web khi tiếp nhận đồng thời nhiều yêu cầu cùng lúc.
- **Thu thập chỉ số hiệu năng cơ bản**: Đo lường thời gian phản hồi (Response Time), tỷ lệ thất bại (Error Rate), thông lượng xử lý (Throughput) và dung lượng truyền tải băng thông.
- **Đánh giá chất lượng dịch vụ (QoS)**: Xác định xem máy chủ có duy trì trải nghiệm mượt mà cho người dùng dưới mức tải mô phỏng hay không.

### 2. Môi trường & Đối tượng thử nghiệm
- **Công cụ kiểm thử**: Apache JMeter (Phiên bản 5.6.3)
- **Giao thức**: HTTPS | **Phương thức HTTP**: `GET`
- **Địa chỉ máy chủ (Target Host)**: `www.google.com` | **Đường dẫn (Path)**: `/`

### 3. Cấu hình kịch bản giả lập tải (Thread Group)
- **Number of Threads (Users)**: `20`
- **Ramp-Up Period (seconds)**: `5` 
- **Loop Count**: `5` 
- **Total Requests (Tổng số mẫu)**: $20 \text{ threads} \times 5 \text{ loops} = 100 \text{ requests}$.

---

## II. Báo cáo & Phân tích Kết quả Chi tiết

### 1. Bảng tổng hợp chỉ số hiệu năng (Aggregate Report)

Dữ liệu đo lường trích xuất trực tiếp từ bộ lắng nghe **Aggregate Report** trên Apache JMeter:

| Metric (Chỉ số) | Giá trị (Value) | Đơn vị | Giải thích & Phân tích Chuyên môn |
| :--- | :---: | :---: | :--- |
| **Total Requests (# Samples)** | **100** | req | Tổng số lượt yêu cầu đã được khởi tạo và gửi tới máy chủ. |
| **Successful Requests** | **100** | req | Số lượng yêu cầu phản hồi hợp lệ (Mã trạng thái HTTP Status 200 OK). |
| **Failed Requests** | **0** | req | Số lượng yêu cầu bị hủy, quá thời gian (Timeout) hoặc trả về lỗi HTTP 4xx/5xx. |
| **Error Rate** | **0.00%** | % | Tỷ lệ lỗi phát sinh trong toàn bộ quá trình thực thi kịch bản (Đạt tuyệt đối). |
| **Average Response Time** | **112** | ms | Thời gian phản hồi trung bình tính trên toàn bộ 100 requests. |
| **Median (Trung vị 50%)** | **90** | ms | 50% số lượng request nhận kết quả phản hồi trong thời gian từ 90 ms trở xuống. |
| **90th Percentile (90% Line)** | **193** | ms | 90% số lượng request nhận kết quả phản hồi trong thời gian từ 193 ms trở xuống. |
| **95th Percentile (95% Line)** | **204** | ms | 95% số lượng request nhận kết quả phản hồi trong thời gian từ 204 ms trở xuống. |
| **99th Percentile (99% Line)** | **224** | ms | 99% số lượng request nhận kết quả phản hồi trong thời gian từ 224 ms trở xuống. |
| **Min Response Time** | **70** | ms | Thời gian phản hồi nhanh nhất thu nhận được từ máy chủ. |
| **Max Response Time** | **337** | ms | Thời gian phản hồi chậm nhất ghi nhận được từ máy chủ. |
| **Throughput (Thông lượng)** | **18.8** | req/sec | Số lượng yêu cầu máy chủ tiếp nhận và xử lý thành công trung bình trong 1 giây. |
| **Received Bandwidth** | **1622.87** | KB/sec | Tốc độ truyền dữ liệu nhận về từ máy chủ trung bình mỗi giây. |
| **Sent Bandwidth** | **2.13** | KB/sec | Tốc độ truyền dữ liệu đẩy lên từ máy khách trung bình mỗi giây. |

---

### 2. Phân tích Chi tiết Kết quả Kiểm tra

1. **Về Độ tin cậy & Tỷ lệ Thành công (Reliability)**:
   - Tỷ lệ lỗi đạt **0.00%** ($100/100$ requests thành công).
   - Toàn bộ các yêu cầu đều phản hồi với mã trạng thái `200 OK`. Máy chủ xử lý ổn định, không ghi nhận tình trạng quá tải, nghẽn mạng hay từ chối dịch vụ under load.

2. **Về Thời gian Phản hồi (Response Time Performance)**:
   - Thời gian phản hồi trung bình cực kỳ ấn tượng (**112 ms**).
   - Chỉ số **Median (90 ms)** và **90th Percentile (193 ms)** chứng minh phần lớn phản hồi đều nằm dưới ngưỡng chuẩn 200 ms (mức tối ưu cho trải nghiệm người dùng Web tương tác).
   - Mức chênh lệch giữa Min (70 ms) và Max (337 ms) thấp, cho thấy độ trễ hàng đợi xử lý của máy chủ được duy trì cân bằng tốt.

3. **Về Năng suất Xử lý (Throughput & Bandwidth)**:
   - Thông lượng đạt **18.8 req/sec**, thể hiện khả năng giải phóng hàng đợi nhanh chóng.
   - Băng thông nhận về đạt **1622.87 KB/sec** phản ánh hạ tầng mạng và dữ liệu nội dung trang chủ được tối ưu tối đa.

---

## III. Hình ảnh Minh họa Kết quả Thực thi

#### 1. Thread Group & HTTP Request
<img width="1920" height="1010" alt="1" src="https://github.com/user-attachments/assets/7846a014-7f49-4f99-b8b7-b092456a7a5a" />


#### 2. Bảng tổng hợp Aggregate Report
<img width="1920" height="1010" alt="2" src="https://github.com/user-attachments/assets/8c2a1eb4-b5ca-4f70-b84e-b9322a0bc51c" />

#### 3. View Results Tree
<img width="1920" height="1014" alt="re" src="https://github.com/user-attachments/assets/14adec85-1033-417e-93b9-45c1beee925e" />

#### 4. Bảng dữ liệu chi tiết (.CSV)
<img width="1266" height="529" alt="Capture" src="https://github.com/user-attachments/assets/7375e8f6-bf63-45b0-aa5e-560ca3875260" />

---

## IV. Kết luận & Đánh giá Tổng quan

- **Kết quả chung**: Kịch bản kiểm thử hiệu năng trên Apache JMeter hoàn thành xuất sắc mục tiêu đề ra.
- **Đánh giá hệ thống**: Máy chủ `www.google.com` hoạt động rất tốt dưới kịch bản mô phỏng 20 người dùng đồng thời, thời gian phản hồi nhanh, băng thông tối ưu và không phát sinh bất kỳ lỗi nào.
