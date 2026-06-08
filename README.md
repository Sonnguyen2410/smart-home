# Smart Home IoT - Yolo:Bit

Hệ thống Smart Home IoT kết hợp Yolo:Bit, MQTT và web dashboard để giám sát môi trường và điều khiển thiết bị từ xa.

## Tổng quan

- Thu thập dữ liệu nhiệt độ, độ ẩm, ánh sáng
- Tự động điều khiển quạt, đèn, cửa theo chế độ AUTO
- Điều khiển thủ công qua dashboard web (chế độ MAN)
- Đồng bộ dữ liệu qua Adafruit IO (MQTT)

## Kiến trúc nhanh

Sensors/Remote -> Yolo:Bit -> MQTT (Adafruit IO) -> Web Backend -> Web Frontend

## Cấu trúc dự án

```text
smart-home/
|-- yolobit/                # Firmware MicroPython + file project OhStem
|-- webapp/backend/         # Node.js + Express + MongoDB
|-- webapp/frontend/        # React + Vite + Tailwind
|-- tests/                  # Mock test không cần phần cứng
|-- docker-compose.yml      # Database local
|-- requirements.txt        # Python dependencies
`-- README.md
```

## Yêu cầu

- Python 3.10
- Node.js 18+
- Docker Desktop
- Tài khoản Adafruit IO
- OhStem App (nếu nạp code Yolo:Bit)

## Bắt đầu nhanh

### 1) Tạo môi trường Python

```bash
py -3.10 -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

### 2) Chạy Backend

```bash
cd webapp/backend
npm install
npm run infra
copy config\.env.example config\.env
npm run dev
```

Backend mặc định: http://localhost:3000

### 3) Chạy Frontend

```bash
cd webapp/frontend
npm install
npm run dev
```

Frontend mặc định: http://localhost:5173

### 4) Cấu hình secrets cho Yolo:Bit

Tạo file local `yolobit/smarthome_secrets.py` từ `yolobit/smarthome_secrets.example.py`:

```python
WIFI_SSID = 'TEN_WIFI'
WIFI_PASSWORD = 'MAT_KHAU_WIFI'
AIO_USERNAME = 'TEN_ADAFRUIT'
AIO_KEY = 'AIO_KEY_CUA_BAN'
```

File này đã được ignore trong git để tránh lộ key.

## Chạy Yolo:Bit

- Nạp các file trong thư mục `yolobit/` lên mạch
- Chạy `main.py`
- Khi LCD hiện `OK` là đã kết nối thành công

Chi tiết từng bước xem tại `yolobit/README.md`.

## Test mock (không cần phần cứng)

```bash
python tests/mock_temperature.py
python tests/mock_ir_receiver.py
```

## Lệnh hữu ích

```bash
# Tắt database local
cd webapp/backend
npm run infra:down
```

## Lưu ý bảo mật

- Không commit file chứa secrets
- Nếu lộ AIO key, cần rotate key ngay trên Adafruit IO

## License

Educational project for IoT learning.
