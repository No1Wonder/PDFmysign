# Cấu trúc PDF liên quan đến chữ ký số

## 1 Tổng quan cấu trúc PDF

PDF là tập hợp các **object** liên kết theo dạng cây (object tree).  
Khi tài liệu có **chữ ký số**, các object quan trọng gồm:

| Object | Vai trò chính | Ghi chú |
|--------|----------------|---------|
| **Catalog (/Root)** | Điểm khởi đầu của tài liệu PDF, trỏ đến Pages, AcroForm, DSS… | /Type /Catalog |
| **Pages tree (/Pages)** | Cấu trúc chứa tất cả các trang. | /Kids`, `/Count |
| **Page object** | Đại diện cho mỗi trang; chứa nội dung và tài nguyên. | /Contents`, `/Resources |
| **Resources** | Danh sách font, hình, XObject dùng trên trang. | /Font, /XObject`, /ProcSet |
| **Content Streams** | Chuỗi lệnh vẽ nội dung trang (text, hình, vùng ký). | Stream có thể nén |
| **XObject** | Đối tượng phụ dạng hình hoặc biểu mẫu (Form XObject). | Có thể dùng cho vùng hiển thị chữ ký |
| **AcroForm** | Mô tả toàn bộ form tương tác (textbox, checkbox, signature field, …). | /Fields chứa các field |
| **Signature Field (Widget)** | Field giao diện của chữ ký trong form. | /FT /Sig, /V → trỏ đến Signature dictionary |
| **Signature Dictionary (/Sig)** | Lưu dữ liệu chữ ký và metadata. | /Type /Sig, /Contents, /ByteRange, /Filter, `/SubFilter |
| **/ByteRange** | Chỉ định vùng byte được ký (4 giá trị offset và length). | Xác định phần không bị thay đổi |
| **/Contents** | Chứa chữ ký số nhị phân (DER-encoded PKCS#7). | Giá trị được ghi sau khi ký |
| **Incremental Update** | Cơ chế ghi nối thêm (append-only) để không phá vỡ vùng đã ký. | Tăng tính toàn vẹn |
| **DSS (Document Security Store)** *(theo PAdES)* | Lưu chứng thư, OCSP, CRL để xác minh lâu dài. | /DSS trong Catalog hoặc Trailer |

---

## 2️ Các object refs quan trọng

| Object ref | Vai trò | Ghi chú |
|-------------|----------|---------|
| `1 0 obj` → `/Catalog` | Gốc tài liệu, trỏ đến `/Pages` và `/AcroForm`. | |
| `2 0 obj` → `/Pages` | Danh sách các trang. | `/Kids` → `[3 0 R]` |
| `3 0 obj` → `/Page` | Trang có nội dung và vùng hiển thị chữ ký. | `/Contents 4 0 R` |
| `4 0 obj` → `/Contents` | Stream nội dung trang. | |
| `5 0 obj` → `/AcroForm` | Chứa `/Fields [6 0 R]`. | |
| `6 0 obj` → `/SigField` | Widget chữ ký. | `/V 7 0 R` |
| `7 0 obj` → `/Sig` (Signature dictionary) | Chứa `/Contents`, `/ByteRange`, `/Filter`, `/SubFilter`, `/M`, `/Name`, `/Reason`,… | |
| `8 0 obj` → `/DSS` | Chứa Cert, OCSP, CRL (nếu là PAdES-LTV). | `/Certs`, `/OCSPs`, `/CRLs` |

---

## 3 Sơ đồ quan hệ object

