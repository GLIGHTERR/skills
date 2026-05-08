# Screen Description: Sign-Up Flow

**Project:** Hotel Partner Sign-Up
**Version:** v1.0
**Date:** 2026-05-07
**Authors:** [Author Name]

---

## Table of Contents
1. [Mô tả Use Case](#mo-ta-use-case)
   1. [Actors](#actors)
   2. [Use Cases](#use-cases)
2. [Screen Description](#screen-description)
   1. [Sign-Up Screen](#image-1-sign-up-screen-step_1png)
   2. [OTP Verification Screen](#image-2-otp-verification-screen-step_2png)
   3. [Hotel Information Screen](#image-3-hotel-information-screen-step_3png)
   4. [Hotel Address Autocomplete Screen](#image-4-hotel-address-autocomplete-screen-step_4png)
   5. [Hotel Media and Description Screen](#image-5-hotel-media-and-description-screen-step_5png)
   6. [Room Registration Screen](#image-6-room-registration-screen-step_6png)
   7. [Room Amenities and Promotion Screen](#image-7-room-amenities-and-promotion-screen-step_7png)
3. [Giả định](#gia-dinh)

---

## 1. Mô tả Use Case

### 1.1 Actors
- **Hotel Partner**: Một chủ khách sạn hoặc quản lý đăng ký khách sạn và các gói phòng trên nền tảng.
- **System**: Nền tảng sign-up khách sạn xác thực dữ liệu đầu vào, gửi mã OTP, lưu trữ thông tin khách sạn và quản lý danh sách phòng.
- **Third-Party Identity Provider**: Dịch vụ xác thực Google, Apple hoặc Facebook dùng cho đăng ký thay thế.

### 1.2 Use Cases

#### UC-001 — Create Hotel Partner Account
- **Description:** The system SHALL allow a hotel partner to register a new account using Full Name, E-mail, and Password.
- **Priority:** High
- **Source:** Screen: Sign-Up Screen (`step_1.png`)
- **Acceptance Criteria:** Khi tên đầy đủ, email và mật khẩu hợp lệ, hệ thống tiếp tục đến xác thực OTP. Khi dữ liệu trường không hợp lệ, hệ thống hiển thị thông báo lỗi từng trường và ngăn không cho gửi.
- **Basic Flow:**
  1. User enters Full Name, E-mail, and Password.
  2. User taps the `Đăng ký` button.
  3. System validates input fields.
  4. If validation succeeds, the system sends a 4-digit OTP code to the provided email and navigates to the OTP screen.
- **Alternate Flows:**
  - **AL-001a:** If the user taps a social login button, the system starts the third-party authentication flow instead of local sign-up.
  - **AL-001b:** If the user taps the back icon, the system returns to the previous screen.
- **Exception Flows:**
  - **EX-001a:** If the email format is invalid, the system displays `Invalid email format` and remains on the sign-up screen.
  - **EX-001b:** If the password does not meet the required strength, the system displays an inline password error and prevents submission.
- **Business Rules:**
  - **BR-001a:** Passwords must meet the platform security policy (minimum length and complexity requirements).
- **Non-functional Requirements:**
  - **NFR-001a:** The account creation request shall complete within 2 seconds under normal load.
  - **NFR-001b:** The sign-up screen shall be accessible and readable on standard mobile screen sizes.

#### UC-002 — Register via Third-Party Provider
- **Description:** The system SHALL allow a hotel partner to register using a supported third-party provider: Google, Apple, or Facebook.
- **Priority:** Medium
- **Source:** Screen: Sign-Up Screen (`step_1.png`)
- **Acceptance Criteria:** Khi xác thực nhà cung cấp thành công, hệ thống tạo hoặc liên kết tài khoản partner và tiếp tục đến OTP verification hoặc bước onboarding tiếp theo.
- **Basic Flow:**
  1. User taps the Google, Apple, or Facebook button.
  2. System launches the selected provider authentication flow.
  3. Upon success, the system retrieves the provider identity and continues onboarding.
- **Alternate Flows:**
  - **AL-002a:** If the provider login is canceled, the system returns to the sign-up screen without creating an account.
- **Exception Flows:**
  - **EX-002a:** If the provider service is unavailable, the system displays `Authentication service unavailable` and asks the user to try again later.
- **Business Rules:**
  - **BR-002a:** The system shall only accept authentication from supported providers.
- **Non-functional Requirements:**
  - **NFR-002a:** Provider authentication flow shall complete within 5 seconds after the provider returns success.

#### UC-003 — Verify OTP Code
- **Description:** The system SHALL verify the 4-digit OTP code sent to the hotel partner's email.
- **Priority:** High
- **Source:** Screen: OTP Verification Screen (`step_2.png`)
- **Acceptance Criteria:** Khi mã 4 chữ số chính xác, hệ thống chuyển sang đăng ký khách sạn. Khi mã sai hoặc hết hạn, hệ thống hiển thị lỗi xác thực.
- **Basic Flow:**
  1. System sends a 4-digit OTP to the user's email.
  2. User enters the code in the four OTP fields.
  3. User taps the `Tiếp tục` button.
  4. System validates the OTP.
  5. If valid, the user advances to hotel registration.
- **Alternate Flows:**
  - **AL-003a:** If the user taps `Gửi lại`, the system re-sends the OTP code to the email.
- **Exception Flows:**
  - **EX-003a:** If the code is expired, the system displays `OTP expired` and prompts the user to request a new code.
  - **EX-003b:** If the code is invalid, the system displays `Invalid code` and remains on the OTP screen.
- **Business Rules:**
  - **BR-003a:** OTP codes shall expire after a short validity period.
- **Non-functional Requirements:**
  - **NFR-003a:** OTP validation shall complete within 2 seconds.
  - **NFR-003b:** The OTP screen shall clearly label the email receiving the code for usability.

#### UC-004 — Register Hotel Basic Information
- **Description:** The system SHALL collect hotel basic information including hotel name, area, address, and star rating.
- **Priority:** High
- **Source:** Screen: Hotel Information Screen (`step_3.png`) and Hotel Address Autocomplete Screen (`step_4.png`)
- **Acceptance Criteria:** Khi tên khách sạn hợp lệ, khu vực được chọn, địa chỉ chọn hợp lệ và số sao được chọn, hệ thống lưu hồ sơ khách sạn và tiếp tục.
- **Basic Flow:**
  1. User enters the hotel name.
  2. User selects a region from the `Khu vực` dropdown.
  3. User enters an address.
  4. System displays autocomplete suggestions.
  5. User selects a suggested address.
  6. User selects a star rating.
  7. User taps `Tiếp tục`.
- **Alternate Flows:**
  - **AL-004a:** If the user selects `Chọn số sao` without choosing a rating, the system prompts for rating selection.
- **Exception Flows:**
  - **EX-004a:** If required fields are empty, the system displays field-specific errors.
  - **EX-004b:** If the address cannot be resolved, the system requests a more specific address entry.
- **Business Rules:**
  - **BR-004a:** Star rating selection shall permit 1 to 5 stars.
  - **BR-004b:** Hotel name and address are required fields.
- **Non-functional Requirements:**
  - **NFR-004a:** Autocomplete suggestions shall appear within 1 second of address input.
  - **NFR-004b:** The hotel information form shall be usable on a mobile device with one-hand input.

#### UC-005 — Upload Hotel Media and Introduction
- **Description:** The system SHALL allow the hotel partner to upload a hotel image and enter a basic introduction.
- **Priority:** Medium
- **Source:** Screen: Hotel Media and Description Screen (`step_5.png`)
- **Acceptance Criteria:** Khi ảnh hợp lệ và mô tả đã nhập, hệ thống chấp nhận dữ liệu và tiếp tục bước kế tiếp.
- **Basic Flow:**
  1. User taps the image upload area.
  2. User selects or uploads an image.
  3. User enters the hotel introduction text.
  4. User taps `Tiếp tục`.
- **Alternate Flows:**
  - **AL-005a:** If the user does not upload an image, the system may allow proceeding if the image is optional, but should warn about missing visual media.
- **Exception Flows:**
  - **EX-005a:** If the image exceeds 10MB or does not meet format requirements, the system displays an error and rejects the upload.
- **Business Rules:**
  - **BR-005a:** Uploaded images shall be a maximum of 10MB and should meet the recommended width of 1600px.
- **Non-functional Requirements:**
  - **NFR-005a:** Image upload feedback shall display upload status within 2 seconds.

#### UC-006 — Register Room Details
- **Description:** The system SHALL allow the hotel partner to register room details including room code, quantity, bed type, area, and standard occupancy.
- **Priority:** High
- **Source:** Screen: Room Registration Screen (`step_6.png`)
- **Acceptance Criteria:** Khi chi tiết phòng hợp lệ, hệ thống lưu hồ sơ phòng và tiếp tục.
- **Basic Flow:**
  1. User enters the room code.
  2. User enters the quantity.
  3. User selects a bed type.
  4. User enters the room area.
  5. User enters the standard occupancy count.
  6. User taps `Tiếp tục`.
- **Exception Flows:**
  - **EX-006a:** If numeric fields contain invalid values, the system displays an error and prevents saving.
  - **EX-006b:** If required fields are empty, the system marks them as required.
- **Business Rules:**
  - **BR-006a:** Quantity and occupancy shall be numeric values greater than zero.
- **Non-functional Requirements:**
  - **NFR-006a:** Room detail validation shall occur immediately after entry.

#### UC-007 — Select Room Amenities and Save
- **Description:** The system SHALL allow the hotel partner to select room amenities, apply a promotion code, and save the room listing.
- **Priority:** Medium
- **Source:** Screen: Room Amenities and Promotion Screen (`step_7.png`)
- **Acceptance Criteria:** Khi tiện ích được chọn và dữ liệu khuyến mãi tùy chọn hợp lệ, hệ thống lưu hồ sơ phòng thành công.
- **Basic Flow:**
  1. User uploads or confirms the room image.
  2. User selects one or more amenities.
  3. User selects a promotion code or taps `Thêm mới` to add one.
  4. User taps `Lưu`.
  5. System saves the room listing and confirms success.
- **Alternate Flows:**
  - **AL-007a:** If the user taps `Thêm mới`, the system opens promo code creation or entry options.
- **Exception Flows:**
  - **EX-007a:** If the selected promotion code is invalid, the system displays a validation error.
- **Business Rules:**
  - **BR-007a:** The system shall only store promotions that are supported for the selected room.
- **Non-functional Requirements:**
  - **NFR-007a:** Saving the room listing shall complete within 3 seconds.
  - **NFR-007b:** Amenity selection shall be clearly visible and tappable on mobile.

---

## 2. Screen Description

### Image 1. Sign-Up Screen: Step 1 (`step_1.png`)

| UI Components | Data Type | Required | Editable | Default | Content |
|---|---|---|---|---|---|
| Back Navigation Icon | Icon | No | No | N/A | Trả người dùng về màn hình onboarding trước đó |
| Screen Title | Text | Yes | No | `Đăng ký` | Chỉ ra hành động sign-up hiện tại |
| Subtitle Text | Text | No | No | `Lorem ipsum dolor sit amet, consectetur` | Cung cấp hướng dẫn ngữ cảnh cho màn hình |
| Full Name Field | Text | Yes | Yes | `Enter your name` | Thu thập tên đầy đủ của hotel partner |
| E-mail Field | Email | Yes | Yes | `Enter your email` | Thu thập email partner để tạo tài khoản và gửi OTP |
| Password Field | Password | Yes | Yes | `Enter your password` | Thu thập mật khẩu tài khoản; bao gồm icon hiển thị/ẩn |
| Password Visibility Icon | Icon/Button | No | Yes | Hidden | Chuyển đổi chế độ hiển thị mật khẩu |
| Sign-Up Button | Button | Yes | No | `Đăng ký` | Gửi form sign-up |
| Separator Text | Text | No | No | `Hoặc đăng nhập bằng` | Chỉ ra các phương thức sign-up thay thế |
| Google Sign-In Button | Button | No | No | Google logo | Khởi tạo xác thực Google |
| Apple Sign-In Button | Button | No | No | Apple logo | Khởi tạo xác thực Apple |
| Facebook Sign-In Button | Button | No | No | Facebook logo | Khởi tạo xác thực Facebook |
| Terms Text | Text/Link | No | No | `By signing up you agree to our Terms and Conditions of Use` | Truyền tải thỏa thuận pháp lý về điều khoản sử dụng |

### Image 2. OTP Verification Screen: Step 2 (`step_2.png`)

| UI Components | Data Type | Required | Editable | Default | Content |
|---|---|---|---|---|---|
| Back Navigation Icon | Icon | No | No | N/A | Trả về màn hình sign-up trước đó |
| Screen Title | Text | Yes | No | `Nhập OTP` | Hướng dẫn người dùng nhập mã xác nhận |
| Subtitle Text | Text | No | No | `We have just sent you 4 digit code via your email example@gmail.com` | Xác nhận nơi mã OTP đã được gửi |
| OTP Input Fields | Numeric | Yes | Yes | `3 3 1 4` | Nhập từng chữ số của mã OTP 4 chữ số |
| Continue Button | Button | Yes | No | `Tiếp tục` | Gửi OTP để xác thực |
| Resend Link | Link | No | No | `Gửi lại` | Yêu cầu gửi lại mã OTP nếu người dùng không nhận được |

### Image 3. Hotel Information Screen: Step 3 (`step_3.png`)

| UI Components | Data Type | Required | Editable | Default | Content |
|---|---|---|---|---|---|
| Back Navigation Icon | Icon | No | No | N/A | Trả về màn hình trước đó |
| Screen Title | Text | Yes | No | `Đăng ký thông tin khách sạn` | Chỉ ra quy trình đăng ký thông tin khách sạn |
| Subtitle Text | Text | No | No | `Lorem ipsum dolor sit amet, consectetur` | Cung cấp ngữ cảnh onboarding |
| Hotel Name Field | Text | Yes | Yes | `Tên khách sạn` | Nhập tên khách sạn hoặc cơ sở |
| Area Dropdown | Dropdown | Yes | Yes | `Chọn khu vực` | Cho phép chọn khu vực hoặc quận |
| Address Field | Text/Autocomplete | Yes | Yes | `Nhập địa chỉ chi tiết` | Nhập địa chỉ khách sạn và kích hoạt gợi ý |
| Star Rating Selector | Rating | Yes | Yes | `Chọn số sao` | Chọn hạng sao khách sạn |
| Continue Button | Button | Yes | No | `Tiếp tục` | Tiến tới màn hình thiết lập khách sạn tiếp theo |

### Image 4. Hotel Address Autocomplete Screen: Step 4 (`step_4.png`)

| UI Components | Data Type | Required | Editable | Default | Content |
|---|---|---|---|---|---|
| Back Navigation Icon | Icon | No | No | N/A | Trả về bước trước |
| Screen Title | Text | Yes | No | `Đăng ký thông tin khách sạn` | Tái sử dụng tiêu đề thông tin khách sạn |
| Subtitle Text | Text | No | No | `Lorem ipsum dolor sit amet, consectetur` | Cung cấp hướng dẫn onboarding tương tự |
| Hotel Name Field | Text | Yes | Yes | `One Love Night` | Hiển thị tên khách sạn đã nhập |
| Area Dropdown | Dropdown | Yes | Yes | `Hà Nội` | Hiển thị khu vực đã chọn |
| Address Field | Text/Autocomplete | Yes | Yes | `Chung cư C16 Bắc Hà` | Nhập địa chỉ và hiển thị gợi ý |
| Address Suggestions List | List | No | No | Multiple address candidates | Cho phép chọn địa chỉ gợi ý hợp lệ |
| Star Rating Selector | Rating | Yes | Yes | `Chọn số sao` | Giữ cho lựa chọn số sao khả dụng khi chọn địa chỉ |
| Continue Button | Button | Yes | No | `Tiếp tục` | Lưu địa chỉ đã chọn và tiếp tục |

### Image 5. Hotel Media and Description Screen: Step 5 (`step_5.png`)

| UI Components | Data Type | Required | Editable | Default | Content |
|---|---|---|---|---|---|
| Back Navigation Icon | Icon | No | No | N/A | Trả về bước trước |
| Screen Title | Text | Yes | No | `Đăng ký thông tin khách sạn` | Tiếp tục onboarding khách sạn |
| Subtitle Text | Text | No | No | `Lorem ipsum dolor sit amet, consectetur` | Cung cấp ngữ cảnh onboarding |
| Image Upload Area | File Upload | No | Yes | Placeholder upload box | Chấp nhận ảnh bìa khách sạn |
| Upload Guidance Text | Text | No | No | `Chiều rộng tối thiểu được khuyến nghị là 1600px. Tối đa 10MB` | Giải thích kích thước ảnh và giới hạn file |
| Hotel Introduction Field | Text Area | Yes | Yes | `Viết giới thiệu một vài điều về khách sạn của bạn...` | Nhập mô tả giới thiệu khách sạn |
| Continue Button | Button | Yes | No | `Tiếp tục` | Tiến tới đăng ký phòng |

### Image 6. Room Registration Screen: Step 6 (`step_6.png`)

| UI Components | Data Type | Required | Editable | Default | Content |
|---|---|---|---|---|---|
| Back Navigation Icon | Icon | No | No | N/A | Trả về màn hình trước đó |
| Screen Title | Text | Yes | No | `Đăng ký hạng phòng` | Chỉ ra việc tạo danh sách phòng |
| Subtitle Text | Text | No | No | `Lorem ipsum dolor sit amet, consectetur` | Cung cấp hướng dẫn ngữ cảnh |
| Room Code Field | Text | Yes | Yes | `Tên khách sạn` | Nhập mã hoặc tên loại phòng |
| Quantity Field | Numeric | Yes | Yes | `5` | Nhập số lượng phòng có sẵn |
| Bed Type Dropdown | Dropdown | Yes | Yes | `Giường đôi` | Chọn loại giường |
| Area Field | Text | Yes | Yes | `30m2` | Nhập diện tích phòng |
| Standard Occupancy Field | Numeric | Yes | Yes | `4` | Nhập số người tiêu chuẩn |
| Continue Button | Button | Yes | No | `Tiếp tục` | Tiến tới bước cấu hình phòng tiếp theo |

### Image 7. Room Amenities and Promotion Screen: Step 7 (`step_7.png`)

| UI Components | Data Type | Required | Editable | Default | Content |
|---|---|---|---|---|---|
| Back Navigation Icon | Icon | No | No | N/A | Trả về màn hình trước đó |
| Screen Title | Text | Yes | No | `Đăng ký hạng phòng` | Tiếp tục cấu hình danh sách phòng |
| Subtitle Text | Text | No | No | `Lorem ipsum dolor sit amet, consectetur` | Cung cấp ngữ cảnh onboarding |
| Room Image Upload Area | File Upload | No | Yes | Placeholder upload box | Chấp nhận ảnh phòng hoặc media |
| Amenities Selection Icons | Toggle Buttons | No | Yes | Multiple options | Chọn tiện ích phòng như an toàn, điều hòa, spa, hồ bơi, gym, v.v. |
| Promotion Dropdown | Dropdown | No | Yes | `Áp dụng` | Chọn mã khuyến mãi hiện có |
| Add New Promotion Button | Button | No | No | `Thêm mới` | Mở tùy chọn thêm mã khuyến mãi mới |
| Save Button | Button | Yes | No | `Lưu` | Lưu danh sách phòng và khuyến mãi đã chọn |

---

## 3. Giả định

- Quy trình là onboarding cho hotel partner chứ không phải đăng ký khách hàng thông thường.
- Xác thực email bằng OTP là bắt buộc trước khi đăng ký thông tin khách sạn và phòng.
- Các tùy chọn social login là lựa chọn thay thế cho đăng ký bằng email/password.
- Dịch vụ autocomplete địa chỉ khách sạn hỗ trợ địa điểm tại Việt Nam và trả về gợi ý phù hợp.
- Ảnh tải lên cần đáp ứng hướng dẫn kích thước hiển thị và được hệ thống xác thực.
- Tính năng tạo khuyến mãi có thể có bước nhập dữ liệu riêng không được thể hiện trong thiết kế hiện tại.

---

Document Version: v1.0 | Page 1 of 1
