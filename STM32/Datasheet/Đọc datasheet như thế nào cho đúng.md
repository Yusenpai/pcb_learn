# Đọc datasheet như thế nào cho đúng ?

Khi thiết kế mạch in (PCB), người thiết kế cần có hiểu biết về IC mà mình muốn thiết kế. Trong datasheet của IC có rất nhiều thông tin, bài viết này sẽ hướng dẫn cách chọn lọc thông tin hiệu quả

## Mục lục

- [Đọc datasheet như thế nào cho đúng ?](#đọc-datasheet-như-thế-nào-cho-đúng-)
	- [Mục lục](#mục-lục)
	- [Datasheet là gì ?](#datasheet-là-gì-)
	- [Thông tin cần nắm khi thiết kế mạch điện cho IC](#thông-tin-cần-nắm-khi-thiết-kế-mạch-điện-cho-ic)

## Datasheet là gì ?

Datasheet là tài liệu mô tả toàn bộ các đặc tính kỹ thuật của một IC, gồm các phần chính:

- **Mô tả chung**: Mô tả chung nhất về con chip; chức năng để làm gì? Các tính năng nổi bật?
- **Về điện** (Absolute Maximum Ratings, DC characteristics, AC characteristics): Các thông số về mặt điện của con chip.
- **Về chức năng** (Functional Description, Overview, block diagram, pin description...): Mô tả chi tiết về chức năng của con chip.
- **Về kiểu đóng gói** (Packaging infomation, ordering infomation): Kiểu đóng gói của IC, kích thước,...

## Thông tin cần nắm khi thiết kế mạch điện cho IC

1. Chức năng: chức năng của IC là gì? Tìm ngay trang đầu
2. Mô tả mỗi chân: Tìm trong phần Pin description.
3. Điện áp hoạt động, dòng điện tiêu thụ, mức điện áp 0,1,...: Tìm trong Absolute Maximum rating, DC characteristics, Electrical characteristics.
4. Chuẩn giao tiếp: Là I2C hay UART hay SPI? Tìm trong phần mô tả chức năng.
5. Mạch mẫu: Tìm phần Typical Application Circuit.
6. Các thông tin khác: tìm trong mạch mẫu, pin description hoặc phải đào sâu vào datasheet