# ACServo-DriverServo-Xinje
Hướng dẫn cài đặt, kết nối FX5U, AC Servo MS6H-60CM30B3-20P4, Driver Servo DS5L1-20P4-PTA

# Tổng quan thông số thiết bị
- Động cơ MS6H-60CM30B3-20P4: Động cơ Servo 400W, điện áp 220V, tốc độ định mức 3000 rpm, quán tính cao, Encoder từ/quang 17-bit.
- Driver DS5L1-20P4-PTA: Driver Servo 400W, nguồn vào AC 220V 1 pha, điều khiển vị trí bằng xung/hướng (Pulse/Dir).
- PLC FX5U (Transistor Output): Điều khiển phát xung tốc độ cao qua các chân Y0, Y1, Y2, Y3.

## BƯỚC 1: KIỂM TRA PHẦN CỨNG & THIẾT BỊ KHI MỚI MỞ HỘP
(Mục đích: Đảm bảo thiết bị đúng mã, không hư hỏng cơ khí trước khi cấp điện)
### 1. Kiểm tra ngoại quan:
- Kiểm tra nhãn (Nameplate) trên Driver và Động cơ khớp với mã mua.
- Trục động cơ quay nhẹ bằng tay (nếu model không có phanh từ đuôi).
### 2. Kiểm tra các phụ kiện đi kèm:
- Giắc nguồn động cơ (4 chân U, V, W, PE).
- Giắc Encoder (cắm vào cổng CN2 trên Driver).
- Giắc điều khiển tín hiệu CN0 (giắc dẹt nhiều chân).

## BƯỚC 2: ĐẤU NỐI ĐỘNG CƠ VÀ NGUỒN CẤP DRIVER (CHƯA NỐI PLC)
(Mục đích: Cấp nguồn an toàn cho Driver và động cơ hoạt động độc lập)
### Đấu dây Nguồn Động cơ (Motor Power Cable):
**1. Đấu 4 dây từ động cơ vào trạm nguồn phía dưới của Driver DS5L1:**
- Dây U (Nâu / Brown) $\rightarrow$ Terminal U 
- Dây V (Đen / Black) $\rightarrow$ Terminal V
- Dây W (Xanh dương / Blue) $\rightarrow$ Terminal W
- Dây PE (Vàng-Xanh / Yellow-Green) $\rightarrow$ Terminal PE (Tiếp địa)
> ⚠️ Lưu ý an toàn: Tuyệt đối không đấu nguồn điện lưới 220V AC vào trạm U, V, W vì sẽ làm cháy hỏng Driver ngay lập tức.

**2. Đấu dây Encoder:**
- Cắm giắc cáp Encoder từ động cơ vào cổng CN2 của Driver.

**3. Đấu Nguồn Cấp cho Driver (Main Power Input):**
- Đấu nguồn 220V AC 1 pha vào 2 trạm L và N của Driver.
- Mặc định với dòng 400W, trạm trở xả P+ và C không cần gắn thêm trở ngoài nếu chưa chạy tải quá nặng.

## BƯỚC 3: KIỂM TRA MÃ ĐỘNG CƠ & CHẠY THỬ ĐỘC LẬP (TEST RUN / JOG RUN)
(Mục đích: Xác nhận động cơ và encoder hoạt động tốt mà chưa cần tới PLC)
> ⚠️ Lưu ý trước khi làm: Đảm bảo cốt trục động cơ đang để tự do, chưa gắn vào tải cơ khí (chưa nối với vít me/dây đai của kho AS/RS).
### 1. Kiểm tra mã động cơ trên Driver
- Nhìn vào nhãn (Nameplate) dán trên thân động cơ MS6H-60CM30B3-20P4 để tìm dòng thông tin Code No. (mã số động cơ).
- Cấp nguồn 220V cho Driver (màn hình hiển thị bb).
- Trên bàn phím của Driver, bấm phím STA/ESC nhiều lần để chuyển sang nhóm thông số giám sát Group U.
- Dùng phím INC / DEC tìm đến thông số U3-00.
- Nhấn và giữ phím ENTER để xem giá trị mã động cơ mà Driver tự động đọc từ Encoder.
  - Nếu U3-00 khớp với mã ghi trên nhãn động cơ: Driver đã nhận diện hoàn toàn chính xác.
  - Nếu báo lỗi E-311/E-316 hoặc không khớp: Cần cài đặt thủ công mã này vào tham số P0-33, sau đó nhấn ENTER và tắt/bật lại nguồn Driver.
### 2. Chạy thử không tải Open-Loop (Chạy kiểm tra pha F1-01)
(Mục đích: Kiểm tra thứ tự đấu dây pha U, V, W và phản hồi tín hiệu Encoder xem có bị ngược hay chập không )
- Bấm phím STA/ESC trên Driver để chuyển sang nhóm chức năng bổ trợ Group F.
- Dùng phím INC / DEC tìm đến hàm F1-01 (Test Run).
- Nhấn và giữ phím ENTER trong khoảng 2 giây cho đến khi màn hình hiển thị trạng thái sẵng sàng test.
- Thao tác:
  - Nhấn giữ phím INC: Trục động cơ sẽ quay theo chiều thuận.
  - Nhấn giữ phím DEC: Trục động cơ sẽ quay theo chiều ngược.
- Quan sát: Nếu thả tay ra động cơ dừng lại, quay mượt mà cả 2 chiều là phần đấu nối U, V, W và Encoder hoàn toàn chuẩn.
(Nếu động cơ bị giật mạnh, giật cục hoặc báo lỗi ngay, cần ngắt nguồn kiểm tra lại thứ tự dây U, V, W ).
### 3. Chạy thử không tải Closed-Loop (Chạy JOG F1-00)
(Mục đích: Chạy thử động cơ ở chế độ vòng kín với tốc độ cố định 100 rpm )
- Từ nhóm hàm F, dùng phím bấm chuyển sang hàm F1-00 (Jog Run).
- Nhấn và giữ phím ENTER khoảng 2 giây. Màn hình LED sẽ chuyển sang trạng thái kích hoạt JOG.
- Thao tác:
  - Nhấn giữ phím INC: Động cơ quay thuận với tốc độ mượt mà 100 rpm.
  - Nhấn giữ phím DEC: Động cơ quay ngược với tốc độ 100 rpm.
- Bấm phím STA/ESC để thoát khỏi chế độ JOG về lại màn hình ban đầu (bb).
> 🛑 TIÊU CHÍ ĐẠT BƯỚC 3 ĐỂ CHUYỂN SANG BƯỚC 4:
> - Mã động cơ tại U3-00 xác nhận đúng mã trên tem nhãn.
> - Động cơ quay mượt cả 2 chiều khi bấm test F1-01 và F1-00.
> - Driver không báo bất kỳ mã lỗi E-xxx nào trên màn hình LED.  
