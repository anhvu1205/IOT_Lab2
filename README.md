# Bài thực hành số 2: Thu thập, lưu trữ và tiền xử lý dữ liệu IoT
**Học phần:** IoT và Ứng dụng (INT14149)  
**Sinh viên thực hiện:** [Trần Lê Anh Vũ - B23DCCN948]

---

## 1. Giới thiệu kiến trúc hệ thống
Hệ thống được thiết kế theo mô hình phân tầng từ thiết bị biên đến đám mây nhằm tối ưu hóa việc thu thập, lưu trữ và phân tích dữ liệu cảm biến thời gian thực:
- **Thiết bị nhúng (ESP32 trên Wokwi):** Giả lập cảm biến môi trường (nhiệt độ, độ ẩm, khoảng cách, RSSI) và publish bản tin dưới định dạng JSON lên MQTT Broker công cộng.
- **MQTT Broker (`broker.emqx.io`):** Trung gian truyền tải thông điệp qua cơ chế Publish/Subscribe.
- **Ingestion Service (`mqtt_to_influx.py`):** Script Python nhận dữ liệu từ MQTT, phân tích cấu trúc và ghi trực tiếp vào InfluxDB Cloud.
- **Time-Series Database (InfluxDB Cloud):** Lưu trữ chuỗi thời gian
- **Preprocessing (`preprocess_data.py`):** Trích xuất dữ liệu, làm sạch giá trị khuyết, lọc ngoại lệ bằng IQR, resample theo cửa sổ thời gian và tạo đặc trưng (`rolling mean`, `delta`).
- **Dashboard:** Giám sát trực quan dữ liệu real-time trên nền tảng InfluxDB Cloud.

---

## 2. Cấu trúc thư mục mã nguồn
```text
IOT-Lab-Practicum-2/
│
├── firmware/
│   └── esp32_sensor_simulator.ino  # Firmware ESP32 phát dữ liệu lên MQTT
│
├── scripts/
│   ├── mqtt_to_influx.py           # Script thu thập dữ liệu từ MQTT vào InfluxDB
│   └── preprocess_data.py          # Script tiền xử lý, lọc outlier và resampling
│
├── data/
│   └── iot_sensor_processed_final.csv # Dữ liệu mẫu sau khi tiền xử lý
│
├── assets/
│   └── dashboard_screenshot.png    # Ảnh chụp màn hình InfluxDB Dashboard
│
└── README.md                       # Tài liệu hướng dẫn sử dụng dự án
