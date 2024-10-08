# Đặt linh kiện trên PCB

## Mục lục

- [Đặt linh kiện trên PCB](#đặt-linh-kiện-trên-pcb)
	- [Mục lục](#mục-lục)
	- [Các bước vẽ PCB](#các-bước-vẽ-pcb)
	- [Đặt linh kiện như thế nào ?](#đặt-linh-kiện-như-thế-nào-)


## Các bước vẽ PCB

- Bước 1: Xác định hình dạng, kích thước board: Nếu board có yêu cầu về hình dạng thì phải vẽ board outline trước. Nếu board không có yêu cầu thì quay lại bước này sau
- Bước 2: Đặt các linh kiện lên PCB.
- Bước 3: Vẽ layout

## Đặt linh kiện như thế nào ?

Các bước đặt linh kiện:
   
1. Gom các linh kiện trong một khối lại với nhau.
   
2. Đặt các linh kiện lớn, chiếm vị trí quan trọng: OLED, nút nhấn, encoder. Ví dụ: OLED là màn hình thì đặt ở giữa, 4 nút nhấn và encoder đặt lùi về bên dưới so với màn hình.
   
3. Đặt các khối vi điều khiển, thạch anh, khối USB, khối nguồn, SPI, I2C. Vị trí các khối này tương ứng với hướng ra chân trên vi điều khiển. Ví dụ: bên phải của vi điều khiển là ra chân cho USB, UART, SPI, bên trái của vi điều khiển là thạch anh, các chân GPIO. Bên trên là CAN, I2C, SWD, 1WIRE. Bên dưới là ngắt cho các nút nhấn. Đặt vi điều khiển ở giữa, các khối khác thì nằm gần và cùng bên so với các chân tương ứng.
   
4. Đặt các linh kiện còn lại.
5. Căn chỉnh cho hợp lý.