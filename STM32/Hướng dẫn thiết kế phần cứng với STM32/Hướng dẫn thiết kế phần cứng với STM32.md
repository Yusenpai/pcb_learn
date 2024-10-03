# Hướng dẫn thiết kế phần cứng với STM32

Bài viết này hướng dẫn thiết kế phần cứng với vi điều khiển STM32F10xxx

## Mục lục

- [Hướng dẫn thiết kế phần cứng với STM32](#hướng-dẫn-thiết-kế-phần-cứng-với-stm32)
	- [Mục lục](#mục-lục)
	- [Tài liệu tham khảo](#tài-liệu-tham-khảo)
	- [Nguồn](#nguồn)
		- [VDD](#vdd)
		- [VDDA](#vdda)
		- [VBAT](#vbat)
		- [Reset](#reset)
	- [Clock](#clock)
		- [HSE](#hse)
		- [LSE](#lse)
	- [Cấu hình khởi động (Boot configuration)](#cấu-hình-khởi-động-boot-configuration)
		- [Lựa chọn chế độ khởi động](#lựa-chọn-chế-độ-khởi-động)
		- [Sơ đồ kết nối](#sơ-đồ-kết-nối)
		- [Bootloader nhúng trên STM32](#bootloader-nhúng-trên-stm32)
	- [Debug và nạp code qua STLink](#debug-và-nạp-code-qua-stlink)
	- [Vẽ mạch in](#vẽ-mạch-in)
		- [Tụ lọc nguồn](#tụ-lọc-nguồn)
		- [Thạch anh ngoài](#thạch-anh-ngoài)

## Tài liệu tham khảo

[1] STMicroelectronics, "Getting started with STM32F10xxx hardware development", AN2586 Application note, Rev 8, December 2022. [Link](an2586-getting-started-with-stm32f10xxx-hardware-development-stmicroelectronics.pdf)

## Nguồn

![alt text](<images/Screenshot 2024-10-02 at 19.47.01.png>)

Thiết bị cần nguồn từ 2.0V - 3.6V để hoạt động. Các khối của vi điều khiển được cấp bởi các nguồn khác nhau, chia làm 3 nguồn chính:

- **VDD**: 2.0V - 3.6V. Cấp nguồn cho CPU, bộ nhớ, ngoại vi,...
- **VDDA**: bằng với VDD. Cấp nguồn cho ADC, cảm biến nhiệt độ, PLL, khối reset. VDDA > 2.4V nếu sử dụng ADC
- **VBAT**: 1.8V - 3.6V. Nguồn dự phòng, cấp cho RTC, LSE,...

### VDD

VDD nguồn cấp cho CPU, bộ nhớ, ngoại vi,... hoạt động ở tần số cao, do đó cần tụ lọc.

Mỗi chân VDD cần 1 tụ lọc 100nF, kiểu 0603 hoặc 0805 (0402 quá nhỏ, không khuyến khích). Thêm 1 tụ lọc từ 4.7uF đến 10uF, kiểu 0603 hoặc 0805 cho toàn bộ IC.

### VDDA

VDDA cấp cho khối ADC. Nguồn này không cần tiêu thụ dòng lớn, nhưng cần ổn định. Có thể dùng mạch lọc pi gồm 2 tụ và 1 ferrite bead để lọc cho VDDA. Tham khảo hình dưới

### VBAT

Nối với nguồn pin dự phòng. Nếu không sử dụng nguồn dự phòng, nối với VDD.

![alt text](<images/Screenshot 2024-10-03 at 20.03.45.png>)

### Reset

Dùng chân **NRST** để reset mạch. NRST là kích hoạt mức thấp. Chân này phải được kéo lên mức cao bằng một điện trở. Có thể kết hợp với nút nhấn và tụ debounce:

![alt text](<images/Screenshot 2024-10-02 at 20.16.17.png>)

## Clock

Vi điều khiển sử dụng xung clock từ nhiều nguồn khác nhau để hoạt động.

Xung clock cho hệ thống gồm:

- HSE (High Speed External): Nguồn clock ngoài hoặc thạch anh ngoài. Giá trị từ 4 - 16MHz đối với STM32F103xx.
- HSI (High Speed Internal): Nguồn clock 8MHz từ mạch RC nội.
- PLL clock: mạch nhân tần số. Nhân tần số lên nhiều lần, với nguồn từ HSE hoặc HSI

Xung clock phụ gồm:
- LSE (Low Speed External): Nguồn clock ngoài từ thạch anh 32.768kHz, cấp cho RTC (mạch thời gian thực). Tuỳ chọn.
- LSI (Low Speed Internal): Nguồn clock 40kHz từ mạch RC nội.

Mỗi nguồn clock có thể bật/tắt khi không sử dụng.

### HSE

HSE là nguồn xung clock tới từ bên ngoài, từ xung clock ngoài hoặc thạch anh. Giá trị thạch anh từ 4-16MHz. Thạch anh kết nối vào các chân **OSC_IN** và **OSC_OUT** của vi điều khiển. CL1 và CL2 là các tụ tải của thạch anh, được tính bằng công thức:

$$C_{L}=\frac{C_{L1}C_{L2}}{C_{L1}+C_{L2}}+C_{stray}$$

Trong đó: 
- $C_{L}$: điện dung tải của thạch anh.
- $C_{stray}$: điện dung kí sinh trên PCB, khoảng 4pF.

Với thạch anh 8MHz, loại đóng gói 3225 có $C_{L} = 8pF$, tính được $C_{L1}=C_{L2}=8pF$. Chọn giá trị gần với số tính được, kiểu đóng gói 0603.

$R_{EXT}$ để làm giảm dòng điện đi vào thạch anh, giá trị từ 0 - 49 Ohm.

![alt text](<images/Screenshot 2024-10-02 at 20.29.55.png>)

Tham khảo:

![alt text](<images/Screenshot 2024-10-03 at 20.23.04.png>)

### LSE

LSE là nguồn xung clock phụ, khi cần sử dụng RTC tích hợp trong STM32. Nguồn này là tuỳ chọn.

## Cấu hình khởi động (Boot configuration)

### Lựa chọn chế độ khởi động

Cấu hình khởi động của STM32F1xxx được lựa chọn bởi chân BOOT0 và BOOT1:
- BOOT1 = x, BOOT0 = 0: Chế độ khởi động bình thường. Vi điều khiển chạy code người dùng trong bộ nhớ Flash.
- BOOT1 = 0, BOOT0 = 1: Chế độ bootloader. Vi điều khiển chạy code bootloader được lập trình sẵn từ nhà sản xuất. Hỗ trợ nạp code người dùng qua UART1 ở chân PA9 và PA10. Tham khảo thêm ở [AN2606](an2606-stm32-microcontroller-system-memory-boot-mode-stmicroelectronics.pdf)
- BOOT1 = 1, BOOT0 = 1: Chạy code được nạp trong SRAM.

![alt text](<images/Screenshot 2024-10-02 at 20.41.20.png>)

Các chân BOOT được đọc **vào thời điểm ngay sau RESET**.

### Sơ đồ kết nối

![alt text](<images/Screenshot 2024-10-03 at 19.32.50.png>)

> Gợi ý: chân BOOT0 thường sử dụng nhiều để chuyển chế độ boot nên thường nối tới nút nhấn hoặc cần gạt. Chân BOOT1 hiếm sử dụng nên thường dùng jumper.

Ở STM32F103C8T6, chân BOOT1 sử dụng chung với chân PB2. Do đó điện trở 10k là bắt buộc để tránh ngắn mạch.

### Bootloader nhúng trên STM32

Khi STM32 đưa vào chế độ bootloader (BOOT1 = 0, BOOT0 = 1), vi điều khiển sẽ chạy code bootloader được lập trình sẵn từ nhà sản xuất. Bootloader này hỗ trợ **lập trình vi điều khiển qua giao thức UART** ở USART1, (PA9 = TX, PA10 = RX).

## Debug và nạp code qua STLink

Tất cả họ vi điều khiển STM32 đều hỗ trợ debug và nạp code thông qua 2 cổng: **Serial Wire Debug (SWD)** và **JTAG**. Trong đó SWD được ưa chuộng hơn. Người ta sử dụng một thiết bị chuyên dụng để debug và nạp code qua SWD, gọi là **STLink**.

SWD gồm 2 tín hiệu:

- SWDIO = PA13
- SWCLK = PA14

Sơ đồ kết nối:

![alt text](<images/Screenshot 2024-10-03 at 19.53.21.png>)

H1 là header 4 chân tiêu chuẩn, mỗi chân cách nhau 2.54mm. Dùng header này khi ta muốn nối dây từ STLink. Chân +3V3 và GND cấp nguồn cho mạch.

## Vẽ mạch in

### Tụ lọc nguồn
Tụ lọc nguồn VDD, VBAT, VDDA: Mỗi tụ đặt sát chân nguồn tương ứng. Các chân VDD của STM32 thường ở cạnh ngay chân VSS. Đặt tụ sao cho hai chân của tụ **gần hai chân VDD và VSS nhất có thể**. Đặt ngay cạnh tụ điện hai **via** (chỉ cần 1 via ở VSS nếu mạch 2 lớp).

![alt text](<images/Screenshot 2024-10-03 at 20.06.30.png>)

### Thạch anh ngoài

Thạch anh là nguồn nhiễu mạnh. Các linh kiện trong khối thạch anh phải đặt sát nhau, làm sao cho dây nối giữa chúng ngắn nhất có thể. Khối thạch anh cách vi điều khiển khoảng 5mm. Phủ đồng GND toàn bộ khu vực phía dưới và xung quanh thạch anh. Không vẽ dây tín hiệu khác đi phía dưới thạch anh.

Tham khảo:

![alt text](<images/Screenshot 2024-10-03 at 20.21.48.png>)

