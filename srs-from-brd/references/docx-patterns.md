# Code Patterns cho SRS docx

File này chứa các đoạn code JavaScript sẵn sàng sử dụng khi sinh SRS.
Đọc file này sau khi đã đọc `/SKILL.md`.

---

## Setup chuẩn

```javascript
const {
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell,
  Header, Footer, AlignmentType, HeadingLevel, BorderStyle, WidthType,
  ShadingType, VerticalAlign, PageNumber, LevelFormat, PageBreak
} = require('docx');
const fs = require('fs');

// Palette màu SRS
const NAVY      = "1F4E79";
const BLUE      = "2E75B6";
const TEAL      = "17A589";   // Use Case header
const ORANGE    = "E67E22";   // Warning / CẦN XÁC NHẬN
const GRAY_BG   = "F2F2F2";
const WHITE     = "FFFFFF";
const HEADER_BG = "2E75B6";

// Content width A4, 1-inch margins
const CONTENT_W = 9360; // DXA
```

---

## Helper functions

```javascript
const border  = { style: BorderStyle.SINGLE, size: 1, color: "CCCCCC" };
const borders = { top: border, bottom: border, left: border, right: border };

// Heading §1, §2, §3
function h1(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_1,
    spacing: { before: 320, after: 120 },
    shading: { fill: HEADER_BG, type: ShadingType.CLEAR },
    indent: { left: 120, right: 120 },
    children: [new TextRun({ text, bold: true, size: 32, color: WHITE, font: "Arial" })],
  });
}

function h2(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_2,
    spacing: { before: 240, after: 80 },
    border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: BLUE } },
    children: [new TextRun({ text, bold: true, size: 26, color: NAVY, font: "Arial" })],
  });
}

function h3(text) {
  return new Paragraph({
    spacing: { before: 160, after: 60 },
    children: [new TextRun({ text, bold: true, size: 22, color: NAVY, font: "Arial" })],
  });
}

// Paragraph thường
function para(text, opts = {}) {
  const { bold = false, italic = false, color = "333333" } = opts;
  return new Paragraph({
    spacing: { before: 60, after: 60 },
    children: [new TextRun({ text, bold, italic, size: 22, font: "Arial", color })],
  });
}

// Bullet list
function bullet(text, level = 0) {
  return new Paragraph({
    numbering: { reference: "bullets", level },
    spacing: { before: 40, after: 40 },
    children: [new TextRun({ text, size: 22, font: "Arial", color: "333333" })],
  });
}

// Ghi chú [CẦN XÁC NHẬN]
function note(text) {
  return new Paragraph({
    spacing: { before: 60, after: 60 },
    shading: { fill: "FEF9E7", type: ShadingType.CLEAR },
    children: [
      new TextRun({ text: "⚠ ", bold: true, size: 22, color: ORANGE, font: "Arial" }),
      new TextRun({ text, italic: true, size: 22, color: ORANGE, font: "Arial" }),
    ],
  });
}

function spacer() {
  return new Paragraph({ spacing: { before: 60, after: 60 }, children: [new TextRun("")] });
}

function pageBreak() {
  return new Paragraph({ children: [new PageBreak()] });
}

// Section divider
function sectionDivider() {
  return new Paragraph({
    border: { bottom: { style: BorderStyle.SINGLE, size: 6, color: BLUE, space: 1 } },
    spacing: { before: 200, after: 120 },
    children: [new TextRun("")],
  });
}

// Generic table helpers
function headerCell(text, width) {
  return new TableCell({
    borders, width: { size: width, type: WidthType.DXA },
    shading: { fill: HEADER_BG, type: ShadingType.CLEAR },
    margins: { top: 80, bottom: 80, left: 120, right: 120 },
    verticalAlign: VerticalAlign.CENTER,
    children: [new Paragraph({
      children: [new TextRun({ text, bold: true, color: WHITE, size: 20, font: "Arial" })]
    })],
  });
}

function dataCell(text, width, shade = false) {
  return new TableCell({
    borders, width: { size: width, type: WidthType.DXA },
    shading: { fill: shade ? GRAY_BG : WHITE, type: ShadingType.CLEAR },
    margins: { top: 80, bottom: 80, left: 120, right: 120 },
    children: [new Paragraph({
      children: [new TextRun({ text, size: 20, font: "Arial", color: "333333" })]
    })],
  });
}

function labelCell(text, width) {
  return new TableCell({
    borders, width: { size: width, type: WidthType.DXA },
    shading: { fill: "EBF5FB", type: ShadingType.CLEAR },
    margins: { top: 80, bottom: 80, left: 120, right: 120 },
    children: [new Paragraph({
      children: [new TextRun({ text, bold: true, size: 20, font: "Arial", color: NAVY })]
    })],
  });
}
```

---

## Use Case Table Builder

```javascript
// Kích thước cột Use Case
const UC_L = 2200;  // Label column
const UC_C = 7160;  // Content column
// Tổng = 9360

function ucHeaderRow(ucId, ucTitle) {
  return new TableRow({
    children: [new TableCell({
      columnSpan: 2,
      borders,
      shading: { fill: TEAL, type: ShadingType.CLEAR },
      width: { size: 9360, type: WidthType.DXA },
      margins: { top: 100, bottom: 100, left: 160, right: 160 },
      children: [new Paragraph({
        children: [new TextRun({ text: `${ucId}: ${ucTitle}`, bold: true, color: WHITE, size: 24, font: "Arial" })]
      })]
    })]
  });
}

function ucRow(label, content, shade = false) {
  return new TableRow({
    children: [
      labelCell(label, UC_L),
      dataCell(content, UC_C, shade),
    ]
  });
}

// Tạo bảng Use Case đơn giản:
function buildUCTable(uc) {
  // uc = { id, title, screen, actor, actorSub, desc, pre, post, mainFlow[], altFlow[], excFlow[], validations[], ac[] }
  const rows = [
    ucHeaderRow(uc.id, uc.title),
    ucRow("Mã màn hình", uc.screen),
    ucRow("Actor chính", uc.actor, true),
    ucRow("Actor phụ", uc.actorSub || "—"),
    ucRow("Mô tả", uc.desc, true),
    ucRow("Điều kiện tiên quyết", uc.pre),
    ucRow("Điều kiện sau (thành công)", uc.post, true),
  ];
  return new Table({
    width: { size: 9360, type: WidthType.DXA },
    columnWidths: [UC_L, UC_C],
    rows,
  });
}

// Basic Flow, Alt Flow, Exception Flow → dùng đoạn văn bản thường với bullets
// (không cần bảng, dễ đọc hơn)
```

---

## Traceability Matrix Table

```javascript
// Widths: FR-ID | Mô tả FR | UC | BR | Màn hình
const TM_W = [1100, 3500, 1300, 1660, 1800]; // tổng 9360

function buildTraceabilityTable(rows) {
  // rows = [{frId, desc, uc, br, screen}, ...]
  return new Table({
    width: { size: 9360, type: WidthType.DXA },
    columnWidths: TM_W,
    rows: [
      new TableRow({
        tableHeader: true,
        children: TM_W.map((w, i) =>
          headerCell(["FR-ID", "Mô tả yêu cầu", "UC liên quan", "BR liên quan", "Màn hình"][i], w)
        )
      }),
      ...rows.map((r, idx) => new TableRow({
        children: [
          dataCell(r.frId, TM_W[0], idx % 2 !== 0),
          dataCell(r.desc, TM_W[1], idx % 2 !== 0),
          dataCell(r.uc, TM_W[2], idx % 2 !== 0),
          dataCell(r.br, TM_W[3], idx % 2 !== 0),
          dataCell(r.screen, TM_W[4], idx % 2 !== 0),
        ]
      })),
    ]
  });
}
```

---

## Document skeleton

```javascript
const doc = new Document({
  styles: {
    default: { document: { run: { font: "Arial", size: 22 } } },
    paragraphStyles: [
      {
        id: "Heading1", name: "Heading 1", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 32, bold: true, font: "Arial", color: WHITE },
        paragraph: { spacing: { before: 320, after: 120 }, outlineLevel: 0 },
      },
      {
        id: "Heading2", name: "Heading 2", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 26, bold: true, font: "Arial", color: NAVY },
        paragraph: { spacing: { before: 240, after: 80 }, outlineLevel: 1 },
      },
      {
        id: "Heading3", name: "Heading 3", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 22, bold: true, font: "Arial", color: NAVY },
        paragraph: { spacing: { before: 160, after: 60 }, outlineLevel: 2 },
      },
    ],
  },
  numbering: {
    config: [
      {
        reference: "bullets",
        levels: [
          { level: 0, format: LevelFormat.BULLET, text: "\u2022", alignment: AlignmentType.LEFT,
            style: { paragraph: { indent: { left: 720, hanging: 360 } } } },
          { level: 1, format: LevelFormat.BULLET, text: "\u25E6", alignment: AlignmentType.LEFT,
            style: { paragraph: { indent: { left: 1080, hanging: 360 } } } },
        ],
      },
      {
        reference: "numbers",
        levels: [
          { level: 0, format: LevelFormat.DECIMAL, text: "%1.", alignment: AlignmentType.LEFT,
            style: { paragraph: { indent: { left: 720, hanging: 360 } } } },
        ],
      },
    ],
  },
  sections: [{
    properties: {
      page: {
        size: { width: 11906, height: 16838 }, // A4
        margin: { top: 1080, right: 1080, bottom: 1080, left: 1080 },
      },
    },
    headers: {
      default: new Header({
        children: [new Paragraph({
          border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: BLUE } },
          spacing: { after: 60 },
          children: [
            new TextRun({ text: "SOFTWARE REQUIREMENTS SPECIFICATION  |  ", bold: true, size: 18, color: NAVY, font: "Arial" }),
            new TextRun({ text: "[Tên hệ thống]  |  v1.0", size: 18, color: "888888", font: "Arial" }),
          ],
        })],
      }),
    },
    footers: {
      default: new Footer({
        children: [new Paragraph({
          border: { top: { style: BorderStyle.SINGLE, size: 4, color: BLUE } },
          alignment: AlignmentType.CENTER,
          spacing: { before: 60 },
          children: [
            new TextRun({ text: "Trang ", size: 18, color: "888888", font: "Arial" }),
            new TextRun({ children: [PageNumber.CURRENT], size: 18, color: "888888", font: "Arial" }),
            new TextRun({ text: " / ", size: 18, color: "888888", font: "Arial" }),
            new TextRun({ children: [PageNumber.TOTAL_PAGES], size: 18, color: "888888", font: "Arial" }),
          ],
        })],
      }),
    },
    children: [
      // Nội dung SRS ở đây
    ],
  }],
});

Packer.toBuffer(doc).then(buf => {
  fs.writeFileSync('/home/glighter/.gemini/skills/srs-from-brd/srs/SRS_Output.docx', buf);
  console.log('Done');
});
```

---

## Lỗi thường gặp khi sinh SRS docx

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| Bảng text bị cắt | `columnWidths` không khớp với `cell.width` | Đảm bảo dual width: `columnWidths` array + từng `cell width` |
| Nền bảng màu đen | Dùng `ShadingType.SOLID` | Thay bằng `ShadingType.CLEAR` |
| Bullet là ký tự lạ | Dùng unicode bullet trực tiếp | Dùng `numbering` config với `LevelFormat.BULLET` |
| Use Case header không full-width | Thiếu `columnSpan: 2` | Thêm `columnSpan` vào header cell |
| Bảng vỡ layout | Tổng `columnWidths` ≠ `width.size` của table | Kiểm tra tổng DXA |
| A4 bị crop | Dùng kích thước US Letter | Dùng `{ width: 11906, height: 16838 }` cho A4 |