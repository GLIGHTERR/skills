# Ví dụ Use Case chuẩn – AirData Hotels App

Tài liệu này là chuẩn mực về **độ chi tiết**, **cách diễn đạt tiếng Việt** và
**cấu trúc** cho mỗi Use Case trong SRS. Khi viết Use Case mới, hãy duy trì
cùng mức độ chi tiết như các ví dụ dưới đây.

---

## UC-02: Đăng ký tài khoản Supplier

| Thuộc tính | Giá trị |
|---|---|
| Mã màn hình | APP-02 |
| Actor chính | Supplier (chưa có tài khoản) |
| Actor phụ | Hệ thống xác thực OTP (SMS/Email) |
| Mô tả | Supplier tạo tài khoản mới trên App, chọn loại supplier, nhập thông tin cá nhân và xác thực để hệ thống tự động cấp Supplier ID. |
| Điều kiện tiên quyết | Supplier đã mở App và chọn "Đăng ký". Email chưa tồn tại trong hệ thống. |
| Điều kiện sau (thành công) | Tài khoản đã được tạo và xác thực. Hệ thống tạo Supplier ID duy nhất. Supplier được chuyển đến màn hình APP-03 (Hotel Basic Info). |
| Điều kiện sau (thất bại) | Tài khoản chưa được tạo. Hệ thống hiển thị thông báo lỗi tương ứng. |

### Luồng cơ bản (Basic Flow)

1. Supplier nhấn nút **"Đăng ký"** trên màn hình Home (APP-01).
2. Hệ thống hiển thị màn hình chọn loại Supplier với 2 tùy chọn:
   - **Chủ sở hữu / Quản lý khách sạn**
   - **Đối tác phân phối dịch vụ khách sạn**
3. Supplier chọn 1 loại và nhấn **"Tiếp tục"**.
4. Hệ thống hiển thị form đăng ký gồm các trường: Họ tên, Email, Số điện thoại, Mật khẩu, Xác nhận mật khẩu, ô chấp nhận Điều khoản sử dụng.
5. Supplier điền đầy đủ thông tin và nhấn **"Tạo tài khoản"**.
6. Hệ thống kiểm tra tính hợp lệ của tất cả trường (xem Validation Rules).
7. Hệ thống gửi mã OTP 6 số về số điện thoại đã đăng ký.
8. Hệ thống chuyển sang màn hình nhập OTP với đồng hồ đếm ngược 120 giây.
9. Supplier nhập mã OTP và nhấn **"Xác nhận"**.
10. Hệ thống xác thực mã OTP thành công.
11. Hệ thống tự động sinh **Supplier ID** duy nhất và gắn với tài khoản.
12. Hệ thống hiển thị thông báo thành công: _"Tài khoản đã được tạo. Supplier ID của bạn: [ID]."_
13. Hệ thống tự động chuyển Supplier đến màn hình APP-03 sau 2 giây.

### Luồng thay thế (Alternative Flow)

**A1 – Xác thực qua Email thay vì SMS:**
- Tại bước 7, nếu Supplier chọn "Xác thực qua email", hệ thống gửi link xác nhận đến email đã đăng ký.
- Supplier mở email và nhấn link.
- Luồng tiếp tục từ bước 10.

**A2 – Gửi lại OTP:**
- Tại bước 9, nếu đồng hồ đếm ngược hết 120 giây mà Supplier chưa nhập OTP, hệ thống hiển thị nút **"Gửi lại mã"**.
- Supplier nhấn "Gửi lại mã". Hệ thống gửi OTP mới và reset đồng hồ đếm ngược.
- Tối đa 3 lần gửi lại trong vòng 15 phút.

### Luồng ngoại lệ (Exception Flow)

**E1 – Email đã tồn tại:**
- Tại bước 6, nếu email đã được đăng ký, hệ thống hiển thị lỗi inline tại field email:
  _"Email này đã được đăng ký. Vui lòng đăng nhập hoặc dùng email khác."_
- Hiển thị link "Đăng nhập" và "Quên mật khẩu".

**E2 – OTP sai:**
- Tại bước 10, nếu OTP không khớp, hệ thống hiển thị:
  _"Mã xác thực không đúng. Vui lòng kiểm tra lại (còn [X] lần thử)."_
- Sau 5 lần sai liên tiếp, khóa 15 phút và hiển thị thông báo tương ứng.

**E3 – Mất kết nối mạng:**
- Nếu mất kết nối ở bất kỳ bước nào, hệ thống hiển thị banner:
  _"Mất kết nối. Dữ liệu đã nhập được lưu tạm. Vui lòng kiểm tra mạng và thử lại."_
- Khi kết nối lại, form hiển thị đúng dữ liệu đã nhập.

### Validation Rules

| Field | Quy tắc | Thông báo lỗi |
|---|---|---|
| Loại Supplier | Bắt buộc chọn 1 trong 2 loại | "Vui lòng chọn loại tài khoản để tiếp tục." |
| Họ tên | Bắt buộc, 2–100 ký tự, không chứa ký tự đặc biệt | "Họ tên không hợp lệ (2–100 ký tự)." |
| Email | Bắt buộc, định dạng email hợp lệ, duy nhất trong hệ thống | "Email không đúng định dạng." / "Email đã được sử dụng." |
| Số điện thoại | Bắt buộc, 9–11 chữ số, bắt đầu bằng 0 hoặc +84 | "Số điện thoại không hợp lệ." |
| Mật khẩu | Bắt buộc, tối thiểu 8 ký tự, có ít nhất 1 chữ hoa, 1 số | "Mật khẩu phải có ít nhất 8 ký tự, gồm chữ hoa và số." |
| Xác nhận mật khẩu | Phải trùng khớp với trường Mật khẩu | "Mật khẩu xác nhận không khớp." |
| Điều khoản | Bắt buộc tích chọn | "Vui lòng đồng ý với Điều khoản sử dụng để tiếp tục." |
| OTP | 6 chữ số, đúng mã được gửi, còn hiệu lực (120s) | "Mã xác thực không đúng." / "Mã xác thực đã hết hạn. Vui lòng gửi lại." |

### Acceptance Criteria

**AC-02-01:** Khi Supplier nhập đầy đủ thông tin hợp lệ và OTP đúng → tài khoản được tạo, Supplier ID được sinh, Supplier được chuyển sang APP-03.

**AC-02-02:** Khi Supplier nhập email đã tồn tại → hệ thống hiển thị thông báo lỗi inline tại field email, không tạo tài khoản trùng.

**AC-02-03:** Khi OTP hết hạn (> 120 giây) → hệ thống không chấp nhận OTP, hiển thị nút "Gửi lại mã".

**AC-02-04:** Khi Supplier nhập OTP sai 5 lần liên tiếp → tài khoản bị khóa 15 phút, hiển thị thông báo rõ ràng.

**AC-02-05:** Khi mất kết nối giữa chừng → dữ liệu đã nhập vào form không bị mất khi kết nối lại.

**AC-02-06:** Supplier ID được sinh đúng format, duy nhất, không trùng với bất kỳ ID nào trong hệ thống.

---

## UC-06: Tạo Room Type

| Thuộc tính | Giá trị |
|---|---|
| Mã màn hình | APP-06 |
| Actor chính | Supplier (đã xác thực, đã hoàn thành APP-03 đến APP-05) |
| Mô tả | Supplier tạo ít nhất 1 room type với đầy đủ thông tin để đủ điều kiện Request Active. |
| Điều kiện tiên quyết | Hotel ID đã được tạo. Supplier đang ở bước Room Setup trong onboarding flow. |
| Điều kiện sau (thành công) | Room type được lưu, gắn với Hotel ID. Checklist cập nhật trạng thái "Hoàn tất" cho bước Room. |

### Luồng cơ bản (Basic Flow)

1. Hệ thống hiển thị màn hình Room Setup với danh sách phòng hiện có (ban đầu rỗng) và nút **"Thêm phòng mới"**.
2. Supplier nhấn **"Thêm phòng mới"**.
3. Hệ thống hiển thị form tạo phòng gồm: Tên hạng phòng, Diện tích (m²), Loại giường, Kích thước giường, Số giường phụ tối đa, Sức chứa tiêu chuẩn (NL/TE), Sức chứa tối đa (NL/TE).
4. Supplier điền thông tin và nhấn **"Tiếp tục"** để sang phần Tiện ích phòng.
5. Hệ thống hiển thị danh sách tiện ích theo danh mục chuẩn (multi-select). Supplier chọn tiện ích và loại view.
6. Supplier nhấn **"Tiếp tục"** sang phần USP phòng.
7. Supplier chọn USP từ danh sách chuẩn và optionally nhập mô tả custom (≤ 150 ký tự).
8. Supplier nhấn **"Tiếp tục"** sang phần Upload ảnh phòng.
9. Supplier upload ít nhất 1 ảnh từ camera hoặc gallery.
10. Supplier nhấn **"Lưu phòng"**.
11. Hệ thống lưu room type và quay về danh sách phòng với room mới hiển thị.
12. Checklist onboarding cập nhật bước "Phòng" thành **Hoàn tất**.

### Luồng thay thế (Alternative Flow)

**A1 – Clone Room:**
- Tại bước 1, nếu đã có ít nhất 1 phòng, Supplier có thể nhấn icon **"Sao chép"** trên card phòng hiện có.
- Hệ thống tạo bản sao với toàn bộ thông tin giống phòng gốc, tên mặc định là "[Tên phòng gốc] – Bản sao".
- Supplier chỉnh sửa các thông tin cần thay đổi và nhấn **"Lưu phòng"**.

**A2 – Lưu nháp giữa chừng:**
- Tại bất kỳ bước nào, Supplier có thể nhấn **"Lưu nháp"**.
- Hệ thống lưu thông tin đã nhập và quay về danh sách phòng. Card phòng hiển thị trạng thái "Chưa hoàn tất".

### Luồng ngoại lệ (Exception Flow)

**E1 – Upload ảnh thất bại:**
- Tại bước 9, nếu ảnh không đúng định dạng hoặc vượt 10MB, hệ thống hiển thị:
  _"Ảnh không hợp lệ. Vui lòng chọn file JPG/PNG dưới 10MB."_
- Nút retry hiển thị cho từng ảnh lỗi.

### Validation Rules

| Field | Quy tắc | Thông báo lỗi |
|---|---|---|
| Tên hạng phòng | Bắt buộc, 2–100 ký tự | "Vui lòng nhập tên hạng phòng." |
| Diện tích | Số dương, tối đa 9999 m² | "Diện tích không hợp lệ." |
| Loại giường | Bắt buộc chọn | "Vui lòng chọn loại giường." |
| Sức chứa tiêu chuẩn NL | Số nguyên dương ≥ 1 | "Sức chứa phải ≥ 1." |
| Sức chứa tối đa NL | Phải ≥ Sức chứa tiêu chuẩn NL | "Sức chứa tối đa phải ≥ sức chứa tiêu chuẩn." |
| Ảnh phòng | Bắt buộc ≥ 1 ảnh, JPG/PNG, ≤ 10MB/ảnh | "Vui lòng upload ít nhất 1 ảnh phòng." |

### Acceptance Criteria

**AC-06-01:** Khi Supplier lưu phòng với đầy đủ thông tin bắt buộc và ít nhất 1 ảnh → room type được tạo, gắn với Hotel ID, hiển thị trong danh sách.

**AC-06-02:** Khi Supplier nhập sức chứa tối đa < sức chứa tiêu chuẩn → hệ thống hiển thị lỗi inline, không cho lưu.

**AC-06-03:** Khi Supplier sử dụng Clone Room → phòng mới có toàn bộ thông tin giống phòng gốc, có thể chỉnh sửa độc lập.

**AC-06-04:** Khi đã có ít nhất 1 room type hợp lệ → bước "Phòng" trong checklist chuyển sang "Hoàn tất".