# Đặc tả Yêu cầu Chức năng (FRS) - Quy trình Đăng ký

**Dự án:** Đăng ký Đối tác Khách sạn
**Phiên bản:** v1.0
**Ngày:** 4 tháng 5, 2026
**Trạng thái:** Nháp

---

## 1. Giới thiệu

### 1.1 Mục đích
Mục đích của tài liệu này là xác định các yêu cầu chức năng cho quy trình Đăng ký nhiều bước cho nền tảng Đối tác Khách sạn. Điều này bao gồm tạo tài khoản người dùng, xác minh danh tính, đăng ký thông tin khách sạn và cấu hình danh mục phòng.

### 1.2 Phạm vi
FRS này bao gồm các tính năng sau:
- Đăng ký người dùng (Email/Mật khẩu và Social Auth)
- OTP xác minh qua email
- Thiết lập hồ sơ khách sạn (Tên, Khu vực, Địa chỉ, Xếp hạng)
- Tải lên phương tiện và mô tả khách sạn
- Cấu hình danh mục phòng (Loại giường, sức chứa, kích thước, tiện ích)

### 1.3 Định nghĩa, Từ viết tắt, Chữ viết tắt
- **OTP**: One-Time Password (Mật khẩu Một lần)
- **FRS**: Functional Requirements Specification (Đặc tả Yêu cầu Chức năng)
- **UI**: User Interface (Giao diện Người dùng)

### 1.4 Tham chiếu
- Tệp thiết kế: `./designs/sign-up/step_1.png` đến `step_7.png`

### 1.5 Quy ước Tài liệu
- Các yêu cầu sử dụng "SHALL" để chỉ hành vi bắt buộc.
- Các yêu cầu sử dụng "SHOULD" để chỉ hành vi được đề xuất nhưng không bắt buộc.

---

## 2. Mô tả Tổng thể

### 2.1 Tổng quan Sản phẩm
Nền tảng cho phép chủ sở hữu/quản lý khách sạn (Đối tác) đăng ký tài sản và danh sách phòng của họ. Quy trình đăng ký là giao diện kiểu wizard hướng dẫn người dùng qua việc tạo tài khoản và thiết lập tài sản ban đầu.

### 2.2 Các Lớp Người dùng và Đặc điểm
- **Đối tác Khách sạn**: Một người dùng sở hữu hoặc quản lý khách sạn và muốn liệt kê nó trên nền tảng.

### 2.3 Giả định và Phụ thuộc
- Hệ thống có quyền truy cập vào dịch vụ email để gửi mã OTP.
- Hệ thống tích hợp với Google, Apple và Facebook cho xác thực xã hội.
- API bản đồ/vị trí có sẵn cho các gợi ý địa chỉ.

### 2.4 Ngoài Phạm vi
- Khôi phục mật khẩu.
- Dashboard.
- Tích hợp cổng thanh toán (cho phí liệt kê, nếu có).

---

## 3. Các Yêu cầu Chức năng

### 3.1 Đăng ký Người dùng
#### 3.1.1 Tổng quan Tính năng
Cho phép đối tác mới tạo tài khoản bằng tên, email và mật khẩu của họ, hoặc qua các social auth provider (Google, Apple, Facebook).

#### 3.1.2 Tương tác Người dùng
- Người dùng nhập chi tiết thủ công.
- Người dùng lựa chọn social auth provider (Google, Apple, Facebook).
- Người dùng đồng ý với điều khoản và điều kiện.

#### 3.1.3 Functional Specifications

| Field | Content |
|---|---|
| **ID** | FS-001 |
| **Feature** | User Registration |
| **Title** | Manual Account Creation |
| **Description** | The system SHALL allow users to create an account by providing Full Name, E-mail, and Password. |
| **Actor** | Hotel Partner |
| **Priority** | High |
| **Trigger** | User clicks the "Đăng ký" (Sign Up) button on the registration screen |
| **Input** | Full Name (string), E-mail (valid email format), Password (string) |
| **Processing** | 1. Validate all fields are filled. 2. Validate email format. 3. Validate password strength (min 8 chars). 4. Check if email is already registered. |
| **Output** | Proceed to OTP Verification screen (SCR-002). |
| **Precondition** | User is on the Sign-Up screen (SCR-001). |
| **Postcondition** | A pending user account is created and an OTP is sent to the provided email. |
| **Basic Flow** | 1. User enters Full Name, E-mail, and Password. 2. User clicks "Đăng ký". 3. System validates inputs. 4. System sends OTP to email. 5. System redirects to SCR-002. |
| **Exception Flows** | 1. If fields are empty, show validation error "Please enter [field name]". 2. If email exists, show error "E-mail already registered". |
| **Business Rules** | Users must agree to Terms and Conditions of Use to register. |
| **Source** | step_1.png |
| **Related Screen(s)** | SCR-001 |

| Field | Content |
|---|---|
| **ID** | FS-002 |
| **Feature** | User Registration |
| **Title** | Social Authentication |
| **Description** | The system SHALL allow users to register using Google, Apple, or Facebook accounts. |
| **Actor** | Hotel Partner |
| **Priority** | High |
| **Trigger** | User clicks a social auth icon (Google, Apple, Facebook) |
| **Input** | Social provider token/profile data |
| **Processing** | 1. Authenticate with the selected provider. 2. Retrieve user profile info (Name, Email). 3. Create/Link account. |
| **Output** | Proceed to Hotel Information screen (SCR-003) if registration is complete. |
| **Source** | step_1.png |
| **Related Screen(s)** | SCR-001 |

### 3.2 Xác thực OTP
#### 3.2.1 Tổng quan Tính năng
Xác minh địa chỉ email của người dùng bằng mã 4 chữ số.

#### 3.2.2 Functional Specifications

| Field | Content |
|---|---|
| **ID** | FS-003 |
| **Feature** | OTP Verification |
| **Title** | OTP Code Entry |
| **Description** | The system SHALL verify the user's email by requiring a 4-digit OTP code sent to their email. |
| **Actor** | Hotel Partner |
| **Priority** | High |
| **Trigger** | User enters the 4th digit or clicks "Tiếp tục" (Continue) |
| **Input** | 4-digit numeric code |
| **Processing** | 1. Validate the entered code against the sent code. 2. Check if the code is expired. |
| **Output** | Redirect to Hotel Information registration (SCR-003) on success. |
| **Exception Flows** | 1. If code is incorrect, show error "Invalid code". 2. If code is expired, show error "Code expired". |
| **Source** | step_2.png |
| **Related Screen(s)** | SCR-002 |

### 3.3 Đăng ký Hồ sơ Khách sạn
#### 3.3.1 Tổng quan Tính năng
Thu thập thông tin cơ bản và phương tiện về tài sản khách sạn.

#### 3.3.2 Functional Specifications

| Field | Content |
|---|---|
| **ID** | FS-004 |
| **Feature** | Hotel Profile |
| **Title** | Basic Hotel Details |
| **Description** | The system SHALL allow the user to enter Hotel Name, Area (dropdown), Detailed Address, and Star Rating. |
| **Actor** | Hotel Partner |
| **Priority** | High |
| **Input** | Hotel Name, Area ID, Address String, Star Rating (1-5) |
| **Processing** | 1. Provide address suggestions as the user types in "Địa chỉ chi tiết". 2. Capture the selected suggestion. |
| **Output** | Proceed to Media Upload (SCR-004). |
| **Source** | step_3.png, step_4.png |
| **Related Screen(s)** | SCR-003 |

| Field | Content |
|---|---|
| **ID** | FS-005 |
| **Feature** | Hotel Profile |
| **Title** | Media and Introduction |
| **Description** | The system SHALL allow the user to upload a hotel cover image and provide a basic introduction. |
| **Actor** | Hotel Partner |
| **Priority** | Medium |
| **Input** | Image file (min 1600px width, max 10MB), Intro text (string) |
| **Processing** | 1. Validate image dimensions and size. 2. Store image and text. |
| **Source** | step_5.png |
| **Related Screen(s)** | SCR-004 |

### 3.4 Cấu hình Danh mục Phòng
#### 3.4.1 Tổng quan Tính năng
Cấu hình các loại phòng cụ thể được cung cấp bởi khách sạn.

#### 3.4.2 Functional Specifications

| Field | Content |
|---|---|
| **ID** | FS-006 |
| **Feature** | Room Category |
| **Title** | Room Specifications |
| **Description** | The system SHALL allow the user to define room category name, quantity, bed type, size, and occupancy. |
| **Actor** | Hotel Partner |
| **Priority** | High |
| **Input** | Room Name, Quantity (int), Bed Type (dropdown), Area (string/int), Occupancy (int) |
| **Source** | step_6.png |
| **Related Screen(s)** | SCR-005 |

| Field | Content |
|---|---|
| **ID** | FS-007 |
| **Feature** | Room Category |
| **Title** | Room Amenities and Promotion |
| **Description** | The system SHALL allow the user to upload room images, select amenities, and apply promotions. |
| **Actor** | Hotel Partner |
| **Priority** | Medium |
| **Input** | Image file, Amenities (multi-select icons), Promotion (dropdown) |
| **Action** | "Lưu" (Save) to complete the room category setup. "Thêm mới" to add another category. |
| **Source** | step_7.png |
| **Related Screen(s)** | SCR-006 |

---

## 4. Mô tả Màn hình & Yêu cầu UI

### 4.1 Screen Inventory
| Screen ID | Screen Name | Linked FS IDs |
|---|---|---|
| SCR-001 | Trang Đăng ký | FS-001, FS-002 |
| SCR-002 | Trang Xác thực OTP | FS-003 |
| SCR-003 | Trang Thông tin Khách sạn Cơ bản | FS-004 |
| SCR-004 | Trang Phương tiện & Giới thiệu Khách sạn | FS-005 |
| SCR-005 | Trang Thông số Phòng | FS-006 |
| SCR-006 | Trang Tiện nghi & Phương tiện Phòng | FS-007 |

### 4.2 Mô tả Màn hình

#### 4.2.1 SCR-001 - Trang Đăng ký
| Field | Content |
|---|---|
| **Screen ID** | SCR-001 |
| **Screen Name** | Sign-Up Page |
| **Purpose** | Initial entry point for registration. |
| **Actors** | Prospective Hotel Partner |
| **Entry Points** | App Launch -> Sign Up link |
| **UI Components** | Full Name field, Email field, Password field (with eye icon), "Đăng ký" button, Google/Apple/Facebook icons, Terms link. |
| **Default State** | All fields empty. |
| **Exit Points** | SCR-002 (on button click), Social Auth redirect. |
| **Linked FS IDs** | FS-001, FS-002 |

#### 4.2.2 SCR-002 - OTP Verification Page
| Field | Content |
|---|---|
| **Screen ID** | SCR-002 |
| **Screen Name** | OTP Verification Page |
| **Purpose** | Email verification. |
| **Actors** | Prospective Hotel Partner |
| **Entry Points** | SCR-001 (Successful manual registration) |
| **UI Components** | 4-digit input circles, "Tiếp tục" button, "Gửi lại" (Resend) link. |
| **Default State** | Empty circles, "Tiếp tục" may be disabled until 4 digits entered. |
| **Exit Points** | SCR-003 (on success) |
| **Linked FS IDs** | FS-003 |

#### 4.2.3 SCR-003 - Hotel Basic Info Page
| Field | Content |
|---|---|
| **Screen ID** | SCR-003 |
| **Screen Name** | Hotel Basic Info Page |
| **Purpose** | Collect primary property details. |
| **Actors** | Hotel Partner |
| **Entry Points** | SCR-002 (Successful OTP), SCR-001 (Social Auth) |
| **UI Components** | Hotel Name field, Area dropdown, Detailed Address (with auto-suggest), Star Rating (1-5 stars). |
| **States** | Empty: No data. Auto-suggest: List of address results shown below address field. |
| **Exit Points** | SCR-004 |
| **Linked FS IDs** | FS-004 |

#### 4.2.4 SCR-004 - Hotel Media & Intro Page
| Field | Content |
|---|---|
| **Screen ID** | SCR-004 |
| **Screen Name** | Hotel Media & Intro Page |
| **Purpose** | Upload property branding and description. |
| **Actors** | Hotel Partner |
| **Entry Points** | SCR-003 |
| **UI Components** | Image upload dropzone (with icon and specs), Introduction textarea. |
| **Exit Points** | SCR-005 |
| **Linked FS IDs** | FS-005 |

#### 4.2.5 SCR-005 - Room Specs Page
| Field | Content |
|---|---|
| **Screen ID** | SCR-005 |
| **Screen Name** | Room Specs Page |
| **Purpose** | Define technical room details. |
| **Actors** | Hotel Partner |
| **Entry Points** | SCR-004 |
| **UI Components** | Room Name field, Quantity field, Bed Type dropdown, Area field, Occupancy field. |
| **Exit Points** | SCR-006 |
| **Linked FS IDs** | FS-006 |

#### 4.2.6 SCR-006 - Room Amenities & Media Page
| Field | Content |
|---|---|
| **Screen ID** | SCR-006 |
| **Screen Name** | Room Amenities & Media Page |
| **Purpose** | Finalize room category with images and amenities. |
| **Actors** | Hotel Partner |
| **Entry Points** | SCR-005 |
| **UI Components** | Image upload dropzone, Amenities grid (An toàn, Điều hòa, Đồ uống, Massage, Spa, etc.), Promotion dropdown, "Thêm mới" button, "Lưu" button. |
| **Exit Points** | Final Completion State (not shown) or repeat SCR-005. |
| **Linked FS IDs** | FS-007 |

---

## 5. Data Requirements

### 5.1 Data Entities and Attributes
- **User**: Name, Email, AuthProvider, Status.
- **Hotel**: UserID, Name, AreaID, DetailedAddress, StarRating, CoverImage, Introduction.
- **RoomCategory**: HotelID, Name, Quantity, BedType, Size, Occupancy, Images, Amenities(List), PromotionID.

### 5.2 Data Validation Rules
- Email: Must be valid format.
- Password: Min 8 characters (assumed).
- Image: Width >= 1600px, Size <= 10MB.
- OTP: 4 numeric digits.

---

## Appendix A: Glossary
- **Partner**: The business user registering their hotel.

## Appendix B: Screen Inventory with Annotations
(Refer to Section 4.1 and 4.2 for details).
