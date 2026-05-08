---
name: srs-from-brd
description: >
  Tạo tài liệu SRS (Software Requirements Specification) hoàn chỉnh bằng tiếng Việt từ một file BRD
  (Business Requirements Document). Skill này đọc BRD đầu vào (định dạng .docx hoặc .pdf), phân tích
  nội dung theo cấu trúc chuẩn, sau đó sinh ra file SRS .docx chuyên nghiệp với đầy đủ: đặc tả
  use case từng màn hình, luồng chính/luồng thay thế/ngoại lệ, acceptance criteria, validation rules,
  business rules, ma trận truy xuất FR↔BRD, và yêu cầu phi chức năng. Kích hoạt skill này bất cứ
  khi nào người dùng đề cập đến: "tạo SRS", "viết SRS từ BRD", "đặc tả phần mềm", "Software
  Requirements Specification", "đặc tả use case", "chuyển BRD sang SRS", hoặc yêu cầu tạo tài liệu
  đặc tả kỹ thuật từ tài liệu yêu cầu nghiệp vụ. Luôn đọc SKILL.md này trước khi bắt đầu, không
  bỏ qua ngay cả khi đã quen với định dạng md hoặc docx.
---

# SRS Generator từ BRD

Sinh tài liệu **Software Requirements Specification (SRS)** tiếng Việt chuẩn từ file BRD.
Output là file `.docx` chuyên nghiệp, sẵn sàng handoff cho Dev & QA.

---

## Quy trình bắt buộc (đọc trước khi làm)

```
Bước 1 → Đọc file BRD (docx hoặc pdf)
Bước 2 → Phân tích nội dung theo Extraction Checklist
Bước 3 → Đọc /SKILL.md để nắm quy tắc tạo docx
Bước 4 → Sinh SRS theo cấu trúc chuẩn bên dưới
Bước 5 → Validate file output
Bước 6 → Copy sang /srs/ và present_files
```

**Không được bỏ qua Bước 3.** Các lỗi phổ biến (WidthType.PERCENTAGE, unicode bullets,
table thiếu dual widths) chỉ được ngăn chặn khi đọc đủ `SKILL.md`.

**Trước khi tới Bước 4 chú ý.** Nếu file BRD với cấu trúc không chuẩn hoặc thiếu một số phần, đọc file `references/brd-to-srs-mapping.md` để biết cách xử lý từng trường hợp cụ thể. Sau khi đã nắm rõ, quay lại Bước 4 để viết SRS theo cấu trúc chuẩn (Xem file `references/docx-patterns.md` để biết thêm chi tiết).

---

## Bước 1 – Đọc file BRD đầu vào

### File .docx
```bash
extract-text /brd/<file>.docx
```

### File .pdf
```bash
python /scripts/pdf_to_text.py /brd/<file>.pdf
# Nếu script không tồn tại, dùng pdftotext:
pdftotext /brd/<file>.pdf -
```

Nếu file đã có nội dung trong context (dạng `<document>`), đọc trực tiếp từ context,
không cần dùng tool.

---

## Bước 2 – Extraction Checklist (phân tích BRD)

Trích xuất các thông tin sau từ BRD trước khi viết SRS.
Nếu một mục không có trong BRD, đánh dấu `[CẦN BỔ SUNG]` trong SRS tương ứng.

```
□ Tên dự án, phiên bản, ngày, người tạo
□ Mục tiêu sản phẩm (Goals)
□ Phạm vi (In-scope / Out-of-scope)
□ Danh sách modules / tính năng chính
□ Người dùng và vai trò (actors)
□ Luồng nghiệp vụ tổng thể (business flow)
□ Từng màn hình / step: tên, mục đích, dữ liệu đầu vào, CTA
□ Validation rules (per field, per step)
□ Business rules (điều kiện block, auto-generate, duplicate check...)
□ Trạng thái hệ thống (status model)
□ Yêu cầu phi chức năng (performance, security, auto-save...)
□ Danh sách sự kiện analytics/event tracking
□ Glossary / thuật ngữ
□ Success metrics / KPI
```

---

## Bước 3 – Cấu trúc SRS chuẩn (13 phần)

### Sơ đồ ánh xạ BRD → SRS

| Phần BRD | Phần SRS tương ứng |
|---|---|
| Executive Summary, Goals | SRS §1 Giới thiệu |
| Project Description, Drivers | SRS §2 Mô tả tổng quan |
| Scope (In/Out) | SRS §2.4 Ràng buộc & §2.5 Giả định |
| Users & Roles | SRS §2.3 Người dùng |
| Business Flow + màn hình | SRS §3 Đặc tả Use Case |
| Functional Requirements | SRS §4 Đặc tả yêu cầu chức năng |
| Business Rules, Validation | SRS §5 Business Rules |
| State Model | SRS §6 Trạng thái hệ thống |
| Non-Functional Requirements | SRS §7 Yêu cầu phi chức năng |
| Success Metrics | SRS §8 Tiêu chí nghiệm thu |
| Glossary | SRS §9 Từ điển thuật ngữ |
| References | SRS §10 Tài liệu tham chiếu |
| Backlog / Appendix | SRS §11 Phụ lục + §12 Traceability Matrix |

---

## Bước 4 – Chi tiết từng phần SRS

### §1. GIỚI THIỆU

```
1.1 Mục đích tài liệu
    – Tài liệu này đặc tả chi tiết các yêu cầu phần mềm cho [Tên hệ thống].
    – Đối tượng người đọc: BA, Dev, QA, UI/UX.
    – Mối quan hệ với BRD: SRS kế thừa và chi tiết hóa BRD [tên file / mã tài liệu].

1.2 Phạm vi hệ thống
    – Tên hệ thống, kênh triển khai (Web / App / API).
    – Tóm tắt tính năng chính (3–5 dòng).

1.3 Định nghĩa và từ viết tắt
    → Lấy từ Glossary trong BRD, bổ sung thêm các thuật ngữ kỹ thuật SRS.
    → Trình bày dạng bảng 2 cột: Thuật ngữ | Giải thích.

1.4 Tài liệu tham chiếu
    – Tên BRD gốc, phiên bản, ngày.
    – Các tài liệu liên quan khác.

1.5 Tổng quan tài liệu
    – Mô tả ngắn cấu trúc của chính tài liệu SRS này.
```

---

### §2. MÔ TẢ TỔNG QUAN HỆ THỐNG

```
2.1 Bối cảnh sản phẩm
    – Hệ thống đứng trong hệ sinh thái nào? Tích hợp với gì?
    – Sơ đồ context (mô tả text nếu không có hình): hệ thống ←→ actor nào.

2.2 Chức năng tổng quan
    – Danh sách module cấp cao, mỗi module 1–2 câu mô tả.
    – Bảng: Module | Mục tiêu | Bắt buộc (Y/N).

2.3 Người dùng và đặc điểm
    – Mỗi actor: tên, mô tả, tần suất sử dụng, môi trường (thiết bị, mạng).
    – Bảng: Actor | Mô tả | Quyền hạn chính.

2.4 Ràng buộc vận hành
    – Out-of-scope items từ BRD.
    – Ràng buộc kỹ thuật (platform, browser, OS, language).

2.5 Giả định và phụ thuộc
    – Các giả định nghiệp vụ khi viết SRS.
    – Phụ thuộc vào hệ thống ngoài (API bên thứ ba, CMS, payment gateway...).
```

---

### §3. ĐẶC TẢ USE CASE

**Đây là phần trọng tâm của SRS.** Mỗi màn hình / luồng chính trong BRD
phải có ít nhất 1 use case.

#### Template Use Case (áp dụng cho mỗi UC)

```
┌─────────────────────────────────────────────────────────────┐
│ UC-[ID]: [Tên use case]                                      │
├─────────────────────────────────────────────────────────────┤
│ Mã màn hình   : [APP-XX / WEB-XX]                            │
│ Actor chính   : [tên actor]                                  │
│ Actor phụ     : [nếu có]                                     │
│ Mô tả         : [1 câu mô tả mục tiêu]                       │
│ Điều kiện tiên: [preconditions – trạng thái hệ thống trước]  │
│ Điều kiện sau : [postconditions – trạng thái sau khi thành]  │
├─────────────────────────────────────────────────────────────┤
│ LUỒNG CƠ BẢN (Basic Flow)                                      │
│  1. Actor thực hiện [hành động]                              │
│  2. Hệ thống [phản hồi / xử lý]                             │
│  3. ...                                                      │
│  N. Use case kết thúc thành công.                            │
├─────────────────────────────────────────────────────────────┤
│ LUỒNG THAY THẾ (Alternative Flow)                            │
│  A1 [Tên nhánh]: Tại bước X, nếu [điều kiện], hệ thống ...  │
├─────────────────────────────────────────────────────────────┤
│ LUỒNG NGOẠI LỆ (Exception Flow)                              │
│  E1 [Tên lỗi]: Hệ thống hiển thị [thông báo lỗi cụ thể].    │
│  E2 ...                                                      │
├─────────────────────────────────────────────────────────────┤
│ VALIDATION RULES                                             │
│  • [Tên field]: [quy tắc validate + thông báo lỗi tiếng VN]  │
│  • ...                                                       │
├─────────────────────────────────────────────────────────────┤
│ ACCEPTANCE CRITERIA                                          │
│  AC-[ID]-01: [Given / When / Then hoặc dạng bullet cụ thể]  │
│  AC-[ID]-02: ...                                             │
└─────────────────────────────────────────────────────────────┘
```

#### ID Convention
- UC-01, UC-02 ... theo thứ tự màn hình
- AC-01-01, AC-01-02 ... (số use case - số AC)
- VR-01-01 ... (validation rules nếu tách riêng)

#### Quy tắc viết Use Case
- Mỗi bước trong Basic Flow bắt đầu bằng chủ thể: **Actor** hoặc **Hệ thống**
- Không dùng jargon kỹ thuật trong Basic Flow (tránh "call API", "query DB")
- Alternative Flow đánh địa chỉ bước gốc: "Tại bước 3..."
- Exception Flow = lỗi / timeout / mạng / quyền truy cập
- Validation Rules viết theo dạng: `[Tên field] — [Rule] — [Thông báo lỗi]`
- Acceptance Criteria phải đủ cụ thể để QA tạo test case ngay

---

### §4. ĐẶC TẢ YÊU CẦU CHỨC NĂNG (Chi tiết)

Tổng hợp dạng bảng từ tất cả FR trong BRD, bổ sung cột UC liên quan:

```
Bảng: ID | Tên yêu cầu | Mô tả chi tiết | Ưu tiên | UC liên quan | Màn hình
```

Mức ưu tiên (giữ nguyên từ BRD hoặc xác định mới):
- **P1 – Bắt buộc**: Không có thì hệ thống không hoạt động
- **P2 – Quan trọng**: MVP có thể thiếu nhưng cần bổ sung sớm
- **P3 – Nên có**: Cải thiện trải nghiệm
- **P4 – Tương lai**: Out of scope hiện tại

---

### §5. BUSINESS RULES

Liệt kê tất cả quy tắc nghiệp vụ từ BRD dưới dạng:

```
BR-[ID]: [Tên rule]
  Mô tả : [Diễn giải rõ ràng]
  Nguồn : [Tham chiếu BRD section]
  Ảnh hưởng: [Màn hình / UC bị ảnh hưởng]
```

Phân nhóm Business Rules:
- **BR-AUTH**: Xác thực & phân quyền
- **BR-DATA**: Toàn vẹn dữ liệu (duplicate check, auto-generate ID...)
- **BR-FLOW**: Điều kiện chuyển trạng thái / block action
- **BR-CALC**: Tính toán (giá, allotment, booking mode...)
- **BR-NOTIFY**: Thông báo & email trigger

---

### §6. TRẠNG THÁI HỆ THỐNG

Nếu BRD có State Model, đặc tả chi tiết hơn:

```
Bảng trạng thái: Trạng thái | Mô tả | Điều kiện vào | Điều kiện ra | Actor
```

Mô tả từng transition:
```
[Trạng thái A] → [Trạng thái B]
  Điều kiện : [...]
  Actor      : [người kích hoạt]
  Action hệ thống: [hệ thống làm gì khi chuyển]
  Thông báo  : [hiển thị gì cho user]
```

---

### §7. YÊU CẦU PHI CHỨC NĂNG

Giữ nguyên từ BRD §Non-Functional, bổ sung ngưỡng đo lường cụ thể:

```
Bảng: ID | Danh mục | Yêu cầu | Ngưỡng đo lường | Cách kiểm tra
```

Các danh mục bắt buộc:
- **Hiệu năng** (response time, load time)
- **Bảo mật** (encryption, auth, OWASP)
- **Khả dụng** (uptime SLA, failover)
- **Khả năng mở rộng** (concurrent users)
- **Khả năng bảo trì** (logging, audit trail)
- **Tương thích** (browser, OS, device)
- **Khả năng phục hồi** (auto-save, data recovery)
- **Tuân thủ** (PDPA, GDPR nếu có)

---

### §8. TIÊU CHÍ NGHIỆM THU (Acceptance Criteria tổng)

Bổ sung từ Success Metrics trong BRD:

```
Bảng: AC-ID | Mô tả tiêu chí | Phương pháp kiểm tra | Pass condition
```

---

### §9. TỪ ĐIỂN THUẬT NGỮ

Kế thừa từ BRD Glossary, bổ sung các thuật ngữ kỹ thuật SRS mới:

```
Bảng: Thuật ngữ | Loại (Nghiệp vụ / Kỹ thuật) | Giải thích | Tham chiếu
```

---

### §10. TÀI LIỆU THAM CHIẾU

```
Bảng: Tên tài liệu | Mã tài liệu | Phiên bản | Ngày | Ghi chú
```

---

### §11. PHỤ LỤC

- **A. Danh sách màn hình đầy đủ** (kế thừa từ BRD §16 hoặc tương đương)
- **B. Event Tracking** (kế thừa từ BRD, bổ sung payload nếu biết)
- **C. Thông điệp hệ thống** (success/error messages dạng bảng)
- **D. Wireflow tham chiếu** (link hoặc mô tả)

---

### §12. MA TRẬN TRUY XUẤT (Traceability Matrix)

Bắt buộc có – giúp QA map test case ↔ yêu cầu:

```
Bảng: FR-ID | Mô tả FR | UC liên quan | BR liên quan | NFR liên quan | Màn hình
```

---

## Bước 5 – Quy tắc viết docx (bắt buộc đọc /SKILL.md)

### Palette màu SRS

```javascript
const NAVY     = "1F4E79";   // Heading chính
const BLUE     = "2E75B6";   // Heading phụ, đường kẻ
const TEAL     = "17A589";   // Highlight Use Case header
const GRAY_BG  = "F2F2F2";   // Row zebra
const WHITE    = "FFFFFF";
const HEADER_BG= "2E75B6";   // Table header background
```

### Bảng Use Case (layout đặc biệt)

Dùng bảng 2 cột cho mỗi use case để dễ đọc:

```javascript
// Cột 1: Label (nhãn), Cột 2: Nội dung
// Width: [2200, 7160] → tổng 9360 DXA (A4 với 1-inch margins)
const UC_LABEL_W   = 2200;
const UC_CONTENT_W = 7160;
const UC_TOTAL     = 9360;
```

Header row của mỗi UC dùng `colspan` giả (merge bằng 1 cell duy nhất full-width):

```javascript
// UC Header row = 1 cell full width, background TEAL
new TableRow({
  children: [new TableCell({
    columnSpan: 2,
    borders, shading: { fill: "17A589", type: ShadingType.CLEAR },
    width: { size: 9360, type: WidthType.DXA },
    margins: { top: 80, bottom: 80, left: 120, right: 120 },
    children: [new Paragraph({
      children: [new TextRun({ text: "UC-01: Tên use case", bold: true, color: WHITE, size: 22, font: "Arial" })]
    })]
  })]
})
```

### Bảng Traceability Matrix

4 cột, font size 18 (nhỏ hơn để vừa trang):

```javascript
const TM_WIDTHS = [1200, 3000, 1800, 1800, 1560]; // tổng 9360
// Columns: FR-ID | Mô tả | UC | BR | Màn hình
```

### Section separator

Thêm đường kẻ trang trí giữa các §:

```javascript
new Paragraph({
  border: { bottom: { style: BorderStyle.SINGLE, size: 6, color: "2E75B6", space: 1 } },
  spacing: { before: 320, after: 160 },
  children: [new TextRun("")]
})
```

---

## Bước 6 – Naming & Output

```
Tên file output: SRS_[TênDựÁn]_[Module]_v[version].docx
Ví dụ         : SRS_AirData_Hotels_App_v1.0.docx

Lưu tại       : /srs/<tên file>.docx
Copy output   : cp <path> /srs/<tên file>.docx
```

---

## Xử lý trường hợp đặc biệt

### BRD không có một số phần

| Thiếu trong BRD | Xử lý trong SRS |
|---|---|
| Validation rules | Suy luận từ mô tả màn hình, đánh dấu `[GIẢ ĐỊNH – CẦN XÁC NHẬN]` |
| State model | Tạo state model từ trạng thái đề cập rải rác trong BRD |
| Performance targets | Ghi `[CẦN BỔ SUNG – Đề xuất: < 3s page load]` |
| Actor ngoài hệ thống | Ghi vào §2.5 Giả định |

### BRD có nhiều module lớn

Nếu BRD cover > 5 module phức tạp, hỏi người dùng:
- Tạo 1 file SRS toàn bộ, hay
- Tạo SRS riêng cho từng module/kênh (Web / App)?

Mặc định: tạo theo kênh nếu BRD có tách Web/App rõ ràng.

### BRD bằng tiếng Anh

Output SRS vẫn bằng **tiếng Việt**. Dịch thuật ngữ nghiệp vụ sang tiếng Việt,
giữ nguyên tên kỹ thuật (tên field, API endpoint, event name) bằng tiếng Anh.

---

## Checklist trước khi deliver

```
□ Tất cả màn hình trong BRD đều có ít nhất 1 Use Case
□ Mỗi Use Case có Main Flow, ít nhất 1 Alternative, ít nhất 1 Exception
□ Mỗi Use Case có Validation Rules và Acceptance Criteria
□ Tất cả Business Rules có BR-ID và tham chiếu BRD
□ Non-Functional Requirements có ngưỡng đo lường cụ thể
□ Traceability Matrix đủ FR-ID ↔ UC ↔ BR ↔ Màn hình
□ File validate PASSED (All validations PASSED!)
□ File đã copy sang /srs/
□ present_files đã được gọi
```

---

## Ví dụ Use Case đầy đủ (tham chiếu)

Xem file `references/uc-example.md` để xem ví dụ Use Case viết sẵn cho
màn hình Đăng ký Supplier (UC-02) lấy từ dự án AirData Hotels App.
Đây là chuẩn mực về độ chi tiết và cách diễn đạt tiếng Việt.