# Vẽ mạch in trên EasyEDA


## Các thao tác cơ bản khi vẽ mạch in

- Alt + P: Tạo PCB mới từ sơ đồ mạch.
- Alt + U: cập nhật từ sơ đồ mạch sang PCB.
- T: chọn lớp top layer
- B: chọn lớp bottom layer
- W: đi dây
- E: vẽ miền đổ đồng (copper area)
- V: vẽ Via

## Các công cụ vẽ

- Track (W): đi dây
- Pad (P): vẽ pad
- Via (V): vẽ via
- Text (S): vẽ văn bản
- Hole: vẽ lỗ
- Copper Area (E): Vẽ miền đổ đồng
- Rectangle: Vẽ hình chữ nhật

![alt text](<images/Screenshot 2024-09-28 at 11.41.49.png>)


## Cấu tạo của PCB

**PCB** (Printed Circuit Board) là mạch in gồm nhiều lớp vật liệu chồng lên nhau. PCB hai lớp là gồm 2 lớp đồng ngăn cách nhau bởi một lớp vật liệu (thường là FR4). PCB 4 lớp là PCB có 4 lớp đồng, ngoài 2 lớp đồng ở mặt trên và mặt dưới thì còn có 2 lớp đồng ẩn bên trong. Ngoài ra còn có các lớp phủ màu (thường là màu xanh lá cây) phủ lên hai lớp đồng để bảo vệ chúng.

![alt text](<images/Screenshot 2024-09-28 at 11.28.30.png>)

![alt text](<images/Screenshot 2024-09-28 at 11.30.45.png>)

Các lớp có thể liên kết với nhau thông qua hai cách: **via** hoặc **pad xuyên lỗ**.

Via là lỗ có kích thước nhỏ, xuyên qua tất cả các lớp để kết nối các dây dẫn theo ý muốn. Pad xuyên lỗ tương tự via, nhưng kích thước lớn hơn và dùng để hàn các chân linh kiện xuyên lỗ.

Trên phần mềm EasyEDA có các lớp tương ứng với PCB thực tế:

- Top layer: lớp đồng mặt trên
- Bottom layer: lớp đồng mặt dưới
- Inner 1,2,3,4: các lớp ẩn bên trong
- Top Silk Layer: Lớp mực in màu trắng trên PCB ở mặt trên
- Bottom Silk Layer: Lớp mực in màu trắng trên PCB ở mặt dưới
- Ratlines: Lớp này chỉ có trên phần mềm. Dùng để dẫn hướng khi đi dây
- Board Outline: Lớp này chỉ có trên phần mềm. Dùng để xác định hình dạng, kích thước của board.
- Document: Lớp này chỉ có trên phần mềm. Dùng để chú thích

![alt text](<images/Screenshot 2024-09-28 at 11.44.17.png>)
## Bước 2: Vẽ outline cho PCB

Chọn lớp Board Outline. Tự vẽ dùng công cụ vẽ hoặc chọn **[Tools] > [Set board Outline...]**

Sau khi vẽ Outline, đặt các lỗ (để gắn board) cho phù hợp.

Sau đó để dành các vị trí phù hợp cho nút nhấn, cổng USB, led,...

## Cài đặt Design Rule

Khi vẽ PCB, ta có thể gặp các trường hợp vẽ dây quá sát nhau, đặt via quá sát nhau, via quá nhỏ, dây dẫn không đủ rộng,... Bên thi công mạch sẽ không thể thi công các mạch này hoặc có thể nhưng chi phí sẽ tăng cao.

Design Rule là công cụ giúp ta hạn chế các lỗi này khi vẽ mạch. Design Rule quy định các thông số:
- Độ rộng tối thiểu của dây (track width)
- Khoảng cách giữa các dây, dây với via, via với via,... (clearance)
- Kích thước via (thông qua Via diameter và Via drill diameter)

Các thông số này lấy từ trang web của nhà sản xuất mạch in. Tham khảo [JLCPCB](https://jlcpcb.com/capabilities/pcb-capabilities).

## Bước 3: Đặt linh kiện lên PCB

Đặt các linh kiện theo thứ tự từ quan trọng nhất trở đi, theo thứ tự sau:
- OLED: vì board nhỏ nên OLED chỉ có thể nằm cố định tại một vị trí
- Cổng USB: đặt sát ở phần rìa để cắm vào cáp USB.
- ESP32: ESP32 có phần antenn. Quay phần antenn này ra rìa board.
- Các nút nhấn: đặt ở rìa board để nhấn
- Các linh kiện khác: 
  - Đặt các linh kiện khác sao cho các linh kiện nằm cùng một khối thì nằm sát nhau.
  - Các điện trở và tụ điện trong khối nguồn sẽ đặt sát gần nhau.
  - Các tụ lọc của ESP32 phải đặt sát chân +3V3 và GND của ESP32. 
  - Các tụ lọc của CH340N phải đặt sát chân nguồn của CH340N. Cả khối CH340N phải đặt gần cổng USB.
  - Đặt linh kiện sao cho chiều đi dây thuận tiện.

## Chỉnh sửa sơ đồ mạch điện, footprint, mô hình 3D nếu cần thiết

Sau khi đặt linh kiện xong, có thể thay đổi sơ đồ mạch điện, footprint, mô hình 3D nếu thấy cần thiết. Ví dụ: Nút nhấn đang sử dụng là nút nhấn theo hướng thẳng đứng, có thể gây khó khăn khi nhấn. Ta thay thế nút này trong sơ đồ mạch điện bằng nút nhấn theo hướng ngang, rồi cập nhật lại PCB.

## Bước 4: PCB layout

Đây là bước nối dây giữa các linh kiện. Nối dây là vẽ một đường dây bằng đồng ở lớp trên (Top layer), lớp dưới (Bottom Layer) hoặc các lớp Inner1234 nếu là PCB nhiều lớp.

### Chọn kích thước dây

Các dây **mang dòng điện nhỏ**, gồm các dây tín hiệu: SDA, SCL, BOOT, EN, 1WIRE, TX, RX, UD+, UD-,... dùng dây có kích thước bé. Kích thước bé bao nhiêu thì dựa trên Design Rule, thông thường là 0.15-0.2-0.3mm.

Các dây **mang dòng điện lớn**, gồm các dây nguồn +3.3V, VBUS, +5V... thì cần kích thước lớn hơn. Kích thước dây càng lớn càng tốt, có thể chọn từ 0.6-0.8-1mm. Dây càng lớn, nhiễu càng được cải thiện.

Dây GND là một ngoại lệ. Ta không cần nối GND, mà dùng kỹ thuật đổ đồng (Copper Area) để nối sau cùng.

### Đi dây

Đi dây theo thứ tự: Dây tín hiệu > Dây nguồn. Ưu tiên vẽ ở mặt trên (top layer) ***nhiều nhất có thể***. Khi nào cần thiết ta sẽ dùng via để vẽ xuống mặt dưới (bottom layer).

Có thể đi theo thứ tự sau:
- SDA, SCL: vì các dây này ngắn, sát ngay OLED.
- 1WIRE: dây này khá ngắn
- USB (UD+ và UD-): các dây này ngắn vì đặt CH340 gần cổng USB. Dây là ***cặp dây vi sai (differential pair)***, cần yêu cầu khắt khe khi vẽ dây. Tuy nhiên, để đơn giản, ta vẽ các dây này làm sao cho ***ngắn nhất có thể***.
- UART (TX và RX): các dây này tương đối dài nên sẽ vẽ sau cùng
- Các dây trong khối nguồn (trừ dây nguồn +3V3, VBUS, +5V)
- Các dây nối tới các nút nhấn
- Các dây cấp nguồn.
- Đổ đồng (vẽ GND)

> Lưu ý: không được vẽ dây và đổ đồng ở gần Antenn của ESP32. Tham khảo [ESP32 hardware design guideline](esp-hardware-design-guidelines-en-master-esp32.pdf)

### Đổ đồng cho GND

Ta dùng công cụ Copper Area để phủ các khu vực trống thành một lớp đồng. Lớp đồng này nối với GND. Do đó khi vẽ dây GND, không cần thiết phải nối dây và chỉ cần đặt một via có net là GND ngay cạnh đó. Khi phủ đồng, lớp đồng sẽ tự động kết nối với via này. Phủ đồng cho cả hai lớp.

Sau khi phủ đồng, ta thêm các via để liên kết hai lớp phủ với nhau. Chọn lớp phủ đồng, trong bảng properties chọn ***Add/Remove Vias***. Điều chỉnh các thông số tương ứng, rồi chọn OK.

### Kiểm tra DRC

Sau khi phủ đồng xong cũng là lúc mạch in đã hoàn thành (về mặt điện). Tuy nhiên, để kiểm tra các lỗi liên quan tới Design Rule, ta chạy DRC ở [Design] > [Check DRC]. Nếu có lỗi xuất hiện, nghĩa là ta đã đi dây không đúng yêu cầu của DRC, cần sửa lại cho đúng.

### Những bước cuối cùng...

Các bạn có thể sửa lớp mực in ở lớp trên và lớp dưới theo ý muốn. Có thể thêm ngày tháng, in logo, tên người làm mạch,...