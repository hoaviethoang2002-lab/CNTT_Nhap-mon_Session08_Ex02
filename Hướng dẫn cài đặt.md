# Hướng dẫn cài đặt cảm biến độ ẩm cho trăng trại lan

##  1. Tổng quan

Cảm biến độ ẩm giúp theo dõi độ ẩm không khí trong nhà lưới, đảm bảo môi trường tối ưu cho Lan phát triển.

### Thiết bị cần chuẩn bị

- Cảm biến DHT22 / AM2302
- Raspberry Pi / Jetson /SoC
- Dây nối Dupont
- Tua vít, băng keo, ốc vít
- Thẻ nhớ đã cài hệ điều hành

----------
## 2. Các bước cài đặt
### 1. Cập nhật hệ thống:
Mở Terminal và cập nhật danh sách gói.
```
sudo apt update && sudo apt upgrade -y
```
### 2. Cài đặt thư viện cho cảm biến DHT22
Cài đặt thư viện đọc dữ liệu cảm biến.
```
sudo apt install -y git python3-pip
sudo pip3 install Adafruit_DHT
```
### 3. Kích hoạt giao tiếp 1-Wire (nếu dùng DHT22)
Bật giao tiếp 1-Wire trên hệ thống.
```
sudo raspi -config
# Chọn: Interface Options >
1-Wire > Enable
```
### 4. Kiểm tra module đã được nhận
Sau khi bật 1-Wire, kiểm tra thiết bị.
```
ls /sys/bus/w1/devices/
# Kết quả có dạng 28-xxxxxx
```
### 5. Chạy thử chương trình đọc cảm biến
Tải và chạy file Python mẫu.
```
git clone ://github.com/adafruit/Adafruit_Python_DHT.git
cd Adafruit_Python_DHT/examples
python3 AdafruitDHT.py 22 4
# 22: DHT22.    4: GPIO pin.
# Kết quả: Nhận kết quả độ ẩm và nhiệt độ.
```
### 6. Tự động chạy khi khởi động (tùy chọn)
Thêm vào crontab để đọc dữ liệu định kỳ.
```
crontab -e
# Thêm dòng (ví dụ mỗi 5 phút):
*/5 * * * * python3 /home/pi/read_dht22.py
```
## 3. Sơ đồ kết nối
|Chân DHT22|Raspberry Pi (BCM)|
|---|--|
|VCC|3.3V (Pin 1)|
|GND|GND (Pin 6)|
|DATA|GPIO4 (Pin 7)|
|NC|Không kết nối|

## 4. Vị trí lắp đặt
Lắp cảm biến ở nơi thoáng, tránh ánh nắng trực tiếp và nước bắn vào.
## 5. Kiểm tra kết quả
Kết quả mẫu khi chạy lệnh ở bước 5:
```
Temp=28.4*   Humidity=67.1%
pi@farm:~ $
```
Ghi chú: Giá trị độ ẩm lý tưởng cho Lan thường từ 60% – 80%. Nếu thấp hơn, cần tăng cường phun sương.
