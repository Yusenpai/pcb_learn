# Vẽ sơ đồ mạch điện trên EasyEDA

## Các bước thiết kế mạch điện

1. Thiết kế sơ đồ mạch điện
2. Xác định kích thước, hình dáng của mạch, gồm tất cả các lỗ gắn và các vị trí quan trọng như LED, nút nhấn, cổng USB...
3. Đặt linh kiện lên PCB
4. PCB layout
5. Hàn PCB, test mạch.

Thiết kế sơ đồ mạch gồm bước 1. Các bước 2, 3, 4 trong phần vẽ mạch in.

## Bước 1: Thiết kế sơ đồ mạch điện

Giả sử bạn đã có ý tưởng về một mạch điện có các yêu cầu sau:
- Vi điều khiển: ESP32 (ESP32-WROOM-32E)
- Hiển thị: Dùng OLED 1.3 inch, giao tiếp I2C
- Đọc nhiệt độ độ ẩm: dùng DHT11, giao tiếp 1WIRE
- Tương tác với người dùng: dùng 1 nút nhấn
- Nạp code, debug, reset: Dùng CH340 để nạp code, nút nhấn boot dùng chung, thêm 1 nút reset
- Cấp nguồn từ USB, hạ áp xuống 3.3V dùng IC AMS1117

Bước tiếp theo là tạo ra một sơ đồ mạch điện như thế này

![alt text](<images/Screenshot 2024-09-28 at 10.07.05.png>)

### Thao tác cơ bản
- Click trái: chọn
- Click trái rồi kéo thả: di chuyển linh kiện trên schematic
- Click phải và kéo: di chuyển schematic trên màn hình
- Click đúp chuột: thay đổi văn bản
- W: nối dây
- N: tạo Net label
- G: vẽ GND
- T: vẽ văn bản

### Wiring Tools

Là bảng công cụ dùng để vẽ sơ đồ mạch.

![alt text](<images/Screenshot 2024-09-28 at 10.11.42.png>)

Có các công cụ:
- Wire: Nối dây
- Net label: Tạo net label
- NetFlag GND: Tạo GND
- Net Port: Tạo một net port
- NetFlag VCC: Tạo VCC
- NetFlag +5V: Tạo +5V
- No connect Flag: Dùng cho các chân không dùng tới.

### Drawing tools
![alt text](<images/Screenshot 2024-09-28 at 10.15.02.png>)

Có các công cụ vẽ cho mục đích chú thích.

### Tìm linh kiện ở đâu ?

Ở phía bên trái màn hình là các tab ***Common Library*** và ***Library***

Common Library chứa các linh kiện cơ bản thường sử dụng, gồm điện trở, tụ điện, đầu header, led,...

![alt text](<images/Screenshot 2024-09-28 at 10.22.55.png>)

Library là thư viện chứa rất nhiều linh kiện từ nhà phân phối LCSC và các các thư viện do người dùng tự tạo.

![alt text](<images/Screenshot 2024-09-28 at 10.24.14.png>)

Khi chọn linh kiện cần chú ý chọn **đúng mẫu mã** và **kiểu đóng gói.**

### Kiểu đóng gói và Footprint

Cùng là một điện trở 1KOhm nhưng có thể có nhiều hình dạng (kiểu đóng gói) khác nhau.

![alt text](<images/Screenshot 2024-09-28 at 10.27.44.png>)

Người ta chia kiểu đóng gói thành hai dạng: **Xuyên lỗ (Through Hole)** và **Dán (SMD)**. Linh kiện xuyên lỗ khi hàn, các chân linh kiện sẽ xuyên từ mặt trên xuống mặt dưới của mạch in. Linh kiện dán khi hàn chỉ nằm trên bề mặt của lớp trên hoặc lớp dưới.

![alt text](<images/Screenshot 2024-09-28 at 10.31.19.png>)

Trong thiết kế mạch in, **Footprint** của một linh kiện là mẫu hay hình dạng của các pad (các miếng đồng) trên mạch in, để gắn linh kiện bằng cách hàn chân linh kiện với các pad đó. 

Ví dụ Footprint của một điện trở xuyên lỗ gồm 2 lỗ được mạ đồng. Còn Footprint của điện trở dán gồm 2 pad đồng hình chữ nhật.
![alt text](<images/Screenshot 2024-09-28 at 10.36.42.png>)
![alt text](<images/Screenshot 2024-09-28 at 10.35.51.png>)

### Vẽ sơ đồ mạch

#### ESP32

Lấy ESP32 ra trước. Khi hoạt động, ESP32 cần nhiều dòng điện, khiến cho nguồn dễ bị nhiễu. Do đó ta thêm các tụ lọc ở gần các chân +3V3 và GND của ESP32.

![alt text](<images/Screenshot 2024-09-28 at 10.40.56.png>)

#### DHT11 và OLED

Lấy các linh kiện: DHT11, OLED. OLED lấy từ phần ***User Contributed***, có thể sẽ không giống với mạch thực tế. Do đó cần kiểm tra kỹ lưỡng. 

Nối các chân SCL, SDA, chân I/O của DHT11 với các chân ESP32. Vì ESP32 có thể điều chỉnh trong phần mềm nên có thể sử dụng bất kỳ chân nào, trừ các chân:

- Sensor_VP, Sensor_VN, IO34, IO35: Các chân chỉ được phép là ngõ vào. Có thể dùng nếu biết cách sử dụng.
- IO0, IO2, IO12, IO15: các chân này là các chân Bootstrap của ESP32. Chúng được sử dụng cho mục đích đặc biệt. Tham khảo [Boot Mode Selection ESP32](https://docs.espressif.com/projects/esptool/en/latest/esp32/advanced-topics/boot-mode-selection.html)
- TXD0, RXD0: Chân UART dùng để nạp code cho ESP32.
- NC: Not Connected. Không dùng các chân này.

> Lưu ý: Giao tiếp I2C có các dây SCL, SDA. Các chân này yêu cầu trở kéo lên để hoạt động.

#### Nạp code dùng CH340N

ESP32 nạp code qua giao thức UART thông qua hai chân TXD0 và RXD0. Ta sử dụng IC CH340N để chuyển từ USB sang UART. TX của CH340N nối RX của ESP32 và ngược lại. 

UD+ và UD- là hai chân truyền dữ liệu trong dây cáp USB. Hai dây này là ***cặp dây vi sai*** (differential pair), cần chú ý khi vẽ các dây này. Sẽ được đề cập tới ở phần vẽ mạch in.

VBUS là chân cấp nguồn từ cáp USB. Nguồn này 5V, nhưng khá nhiễu. Phần này sẽ được xử lý trong khối nguồn.

#### Sử dụng Net label và Net Port

Các chân linh kiện có thể nối với nhau mà không cần vẽ dây nối trực tiếp, bằng cách sử dụng Net Label. Net Label đặt tên cho một dây. Các dây trùng tên thì được hiểu là nối với nhau.

Net Port có công dụng tương tự, nhưng hình dáng khác.

#### Reset và Boot

Để reset ESP32, ta dùng một nút nhấn nối với chân EN. Khi chân EN được kéo xuống mức thấp, ESP32 reset.

Chân IO0 được dùng để thay đổi chế độ boot. Ngay sau khi Reset, ESP32 sẽ đọc chân IO0:
- Nếu IO0 = 1, ESP32 chạy bình thường.
- Nếu IO0 = 0, ESP32 vào chế độ nạp code. Lúc này các bạn bấm nạp code trên máy tính.

Ta cũng nối IO0 với nút nhấn. Các trở và tụ nối với nút nhấn nhằm debounce cho nút nhấn. Tham khảo ở [Debounce nút nhấn bằng tụ điện](http://arduino.vn/bai-viet/166-debounce-cho-nut-nhan-bang-tu-dien)

#### Khối nguồn

Nguồn tới từ USB thông qua chân VBUS là 5V. Tuy nhiên ESP32 sử dụng nguồn 3.3V. Cần có mạch hạ áp và lọc nguồn.

VBUS được lọc qua C7, C8 và L1. L1 là một viên Ferrite, có tác dụng ngăn cản dòng điện ở tần số cao, do đó chặn được nhiễu. Tham khảo tại [Ferrite Bead](https://en.m.wikipedia.org/wiki/Ferrite_bead). Tụ C9 và C10 để lọc nguồn ngõ vào và ngõ ra cho IC hạ áp AMS1117.

> Lưu ý: Trong video thiếu Flag +3V3 ở khối Power.

![alt text](<images/Screenshot 2024-09-28 at 10.14.10.png>)]

### Kiểm tra lại mạch điện

Đây là bước rất quan trọng trước khi vẽ mạch in. Kiểm tra từng khối và trả lời các câu hỏi sau:

- Cấp đúng nguồn cho các linh kiện chưa ?
- Thêm tụ lọc đầy đủ chưa ?
- Nối đúng chân ESP32 chưa ? Có nối nhầm vào chân Strap hoặc Input only không ?
- Nối chéo các chân UART (TX, RX) chưa ?
- Có trở kéo lên cho I2C chưa ?
- Nối đúng các chân D+ và D- của USB chưa ?
- Khối nguồn có thêm các Flag +5V, +3V3 đầy đủ chưa ?
- Đã thêm đèn led báo hiệu chưa. Nếu chưa hãy sửa khối nguồn thành thế này:
![alt text](<images/Screenshot 2024-09-28 at 11.20.56.png>)
Led và trở chọn loại 0603.