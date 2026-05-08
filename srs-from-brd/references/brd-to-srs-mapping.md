# Mapping chi tiết BRD → SRS

Tài liệu này là hướng dẫn chi tiết cách chuyển đổi từng phần BRD sang SRS.
Đọc khi cần xử lý BRD với cấu trúc không chuẩn hoặc thiếu một số phần.

---

## Quy tắc chung

1. **Luôn giữ nguyên ý nghĩa nghiệp vụ** — SRS chi tiết hóa, không thay đổi yêu cầu.
2. **Đánh dấu giả định rõ ràng** — Bất cứ điều gì suy luận (không có trong BRD) đều ghi `[GIẢ ĐỊNH – CẦN XÁC NHẬN]`.
3. **Không bỏ bất kỳ màn hình nào** — Mỗi screen ID trong BRD phải có ít nhất 1 Use Case trong SRS.
4. **FR-ID kế thừa** — Giữ nguyên FR-ID từ BRD nếu có, bổ sung thêm bằng FR-NEW-XX.
5. **BR-ID mới** — Business Rules trong SRS dùng prefix `BR-` với nhóm con.

---

## Mapping chi tiết theo phần BRD

### BRD §1 Executive Summary → SRS §1.1 + §1.2

| BRD Content | SRS Destination | Ghi chú |
|---|---|---|
| Tên dự án, phiên bản, ngày | Cover page + §1.1 | Copy nguyên |
| Mục đích tài liệu BRD | SRS §1.1 (đổi thành mục đích SRS) | Viết lại cho SRS |
| Tóm tắt sản phẩm | SRS §1.2 Phạm vi hệ thống | Rút gọn 3–5 dòng |

### BRD §2 Project Description → SRS §2.1

| BRD Content | SRS Destination | Ghi chú |
|---|---|---|
| Bối cảnh (Context) | SRS §2.1 Bối cảnh | Giữ nguyên |
| Present Process | SRS §2.1 (paragraph đầu) | Mô tả vấn đề hiện tại |
| Proposed Process | SRS §2.2 Chức năng tổng quan | Phác thảo giải pháp |

### BRD §3 Scope → SRS §2.4 + §2.5

| BRD Content | SRS Destination | Ghi chú |
|---|---|---|
| In-scope items | SRS §2.2 Chức năng tổng quan (table Module) | Thành danh sách module |
| Out-of-scope items | SRS §2.4 Ràng buộc | Liệt kê rõ |
| Giả định | SRS §2.5 Giả định và phụ thuộc | Copy và bổ sung |

### BRD §4 Business Drivers → SRS §2.1

Không tạo section riêng trong SRS. Gộp vào §2.1 Bối cảnh như "động lực của dự án".

### BRD §5 Users → SRS §2.3

| BRD Content | SRS Destination |
|---|---|
| Mỗi nhóm người dùng | 1 row trong bảng Actor |
| Đặc điểm người dùng | Cột "Đặc điểm" trong bảng Actor |

### BRD §6 Business Flow → SRS §3 Use Case (QUAN TRỌNG NHẤT)

Đây là mapping phức tạp nhất. Quy tắc:

```
Mỗi BƯỚC trong business flow ≥ 1 Use Case
Mỗi MÀN HÌNH trong danh sách (§16 BRD) ≥ 1 Use Case

Nếu 1 màn hình có nhiều hành động phức tạp → tách thành nhiều UC
Ví dụ:
  APP-06 Room Setup → UC-06a: Tạo Room Type
                    → UC-06b: Clone Room Type
                    → UC-06c: Chỉnh sửa Room Type
                    → UC-06d: Xóa Room Type
```

**Cách tạo Use Case từ mô tả bước trong BRD:**

```
BRD mô tả: "Tạo room type: tên, diện tích, loại giường, sức chứa, ảnh phòng"

→ UC Title    : "Tạo Room Type mới"
→ Actor       : Supplier
→ Basic Flow   : Viết chi tiết từng tương tác (mở form → nhập → upload → lưu)
→ Alt Flow    : Save draft, clone từ phòng khác
→ Exception   : Upload fail, validation fail, mạng chập chờn
→ Validation  : Từng field trong mô tả BRD + suy luận thêm từ validation rules §9 BRD
→ AC          : 1 AC per validation rule + AC happy path
```

### BRD §7–§8 Functional Requirements → SRS §4

Copy toàn bộ bảng FR, thêm cột:
- **UC liên quan**: điền UC-ID tương ứng
- **Mô tả chi tiết**: mở rộng nếu BRD quá ngắn
- **Ghi chú kỹ thuật**: constraint kỹ thuật nếu biết

### BRD §9 Business Rules / Validation → SRS §5

Trích xuất tất cả validation và business rules rải rác trong BRD (thường ở cuối mỗi Step).
Chuẩn hóa thành BR-ID và phân nhóm:

```
Từ BRD: "Không cho Request Active nếu chưa có ít nhất 1 room type"
→ BR-FLOW-01: Block Request Active khi chưa đủ điều kiện Room
  - Mô tả: Hệ thống không cho phép Supplier nhấn Request Active nếu chưa tạo ít nhất 1 room type có ảnh.
  - Nguồn: BRD §9 Business Rules
  - Ảnh hưởng: UC-10 (Review Summary), APP-10
```

### BRD §10 Status Model → SRS §6

Mở rộng bảng trạng thái BRD bằng cách thêm:
- Actor kích hoạt transition
- Action hệ thống khi chuyển trạng thái
- Thông báo hiển thị cho user

### BRD §11 Non-Functional Requirements → SRS §7

Giữ nguyên các NFR, thêm cột:
- **Ngưỡng đo lường** (nếu BRD chưa có): đề xuất con số cụ thể
- **Cách kiểm tra**: unit test / load test / manual / monitoring

### BRD §12 Analytics Events → SRS §11 Phụ lục B

Copy nguyên danh sách event tracking vào Phụ lục B.
Nếu biết payload, bổ sung thêm.

### BRD §13 Success Metrics → SRS §8 Tiêu chí nghiệm thu

Chuyển metric thành Acceptance Criteria có thể đo được:

```
BRD metric: "Tỷ lệ hoàn tất onboarding >= 60%"
→ AC-SYS-01: Đo tỷ lệ [request_active_success] / [verify_success] trong 30 ngày đầu.
             Pass condition: >= 60%
             Công cụ đo: Analytics event dashboard
```

### BRD §14–§16 Wireflow + Screen List → SRS §11 Phụ lục A

Copy danh sách màn hình vào Phụ lục A. Thêm cột:
- **UC liên quan**: điền UC-ID

---

## Xử lý BRD thiếu phần

| Thiếu | Cách xử lý |
|---|---|
| Thiếu validation rules rõ ràng | Suy luận từ field type + business context, ghi `[GIẢ ĐỊNH]` |
| Thiếu state model | Tìm các từ khóa trạng thái rải rác, tự tạo bảng, ghi `[SRS TỰ XÂY DỰNG – CẦN XÁC NHẬN]` |
| Thiếu NFR cụ thể | Đề xuất NFR tiêu chuẩn ngành (< 3s load, 99.5% uptime...) với `[ĐỀ XUẤT]` |
| Thiếu performance target | Ghi `[CẦN BỔ SUNG – Đề xuất: < 3s page load, < 500ms API]` |
| Thiếu màn hình error/empty/loading | Tạo Use Case supplementary cho từng trạng thái, ghi `[BỔ SUNG UX]` |
| BRD chỉ đề cập 1 dòng về 1 tính năng | Viết Use Case ở mức độ trung bình, ghi `[CHI TIẾT CẦN XÁC NHẬN VỚI BA]` |

---

## Checklist mapping hoàn tất

```
□ Tất cả screen ID trong BRD §16 (hoặc tương đương) → đã có UC
□ Tất cả FR-ID trong BRD → đã xuất hiện trong Traceability Matrix
□ Tất cả business rules trong BRD §9 → đã có BR-ID trong SRS §5
□ Tất cả validation rules trong BRD → đã xuất hiện trong UC tương ứng
□ Tất cả trạng thái trong BRD §10 → đã có transition đầy đủ trong SRS §6
□ Không có `[GIẢ ĐỊNH]` nào bị bỏ qua không đánh dấu
```