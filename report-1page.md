# FIT4012 Lab 7 - Báo cáo 1 trang: SHA-256

## 1. Mục tiêu / Objective
Tìm hiểu, phân tích và thực hành thuật toán băm SHA-256. Ứng dụng hàm băm vào các bài toán thực tế bao gồm kiểm tra tính toàn vẹn của tập tin (file integrity), băm mật khẩu cơ bản và sử dụng salt (muối) để tăng cường độ an toàn.

## 2. Cách làm / Approach
- Biên dịch các tệp mã nguồn bằng `Makefile` để tạo các file thực thi cho tính năng băm SHA-256 và kiểm tra mật khẩu.
- Chạy các kịch bản kiểm thử tự động (`make test`) để xác minh tính chính xác của hàm băm thông qua known answer test vector, tamper test và negative test cho mật khẩu.
- Trích xuất dữ liệu từ tệp log chạy thử nghiệm (`logs/generated-sample-run.log`) để lấy minh chứng thực tế đưa vào báo cáo.

## 3. Kết quả / Result
Dựa trên log minh chứng hệ thống xuất ra:
- **Hash của chuỗi `abc`:** `ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad`.
- **Hash của file mẫu trước khi sửa:** `5ee62dc925a9958dbd6732c570a23c7f65a8c11066e889b15068cfb4bf1a0bd9`.
- **Kết quả kiểm tra file sau khi sửa nội dung:** Hệ thống báo lỗi `[FAIL] File was changed or expected hash is incorrect` với hash thực tế đổi thành `c232e5627e703ee5b311e0df8520b3d10dc8867b27636bb58fe58eb1fb9d6acb`, nhận diện thành công việc chỉnh sửa file `[PASS] Tamper case detected`.
- **Kết quả đăng nhập với mật khẩu đúng:** Báo cáo thành công `[PASS] Login success`.
- **Kết quả đăng nhập với mật khẩu sai:** Báo lỗi `[FAIL] Login failed: wrong password` và từ chối đúng như mong đợi `[PASS] Wrong password rejected`.
- **Hai bản ghi `salt:hash` của cùng một mật khẩu có giống nhau không?**: Hoàn toàn khác nhau. Hệ thống xác nhận `[PASS] Same password produced different salted records` nhờ việc sử dụng salt ngẫu nhiên cho mỗi lần băm.

## 4. Kết luận / Conclusion
- **SHA-256 giúp phát hiện thay đổi dữ liệu như thế nào?**: Bất kỳ thay đổi nào dù nhỏ nhất ở đầu vào đều thay đổi hoàn toàn mã băm đầu ra (hiệu ứng tuyết lở). Đối chiếu hash mới với hash cũ sẽ phát hiện ngay lập tức tập tin đã bị can thiệp.
- **Vì sao cần salt khi lưu hash mật khẩu?**: Giúp hai người dùng có chung mật khẩu tạo ra hai bản ghi hash hoàn toàn khác nhau, ngăn chặn triệt để tấn công bằng bảng băm (rainbow tables).
- **Vì sao SHA-256 demo trong lab chưa nên dùng trực tiếp cho hệ thống xác thực thật?**: Tốc độ tính toán của SHA-256 rất nhanh, khiến kẻ tấn công dễ dàng dò quét (brute-force). Hệ thống backend thực tế cần thuật toán chuyên dụng để làm chậm quá trình băm như bcrypt hoặc Argon2.