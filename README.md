# Báo cáo phân tích kiến trúc Web Service
## Lựa chọn SOAP cho Legacy Core Banking và REST cho Microservices

> **Bối cảnh:** Doanh nghiệp tài chính đang vận hành hệ thống Core Banking cũ, đồng thời muốn phát triển các dịch vụ mới theo hướng Microservices phục vụ Web/Mobile.  
> **Kết luận chính:** Nên dùng **SOAP cho hệ thống Legacy/Core Banking** và **REST cho hệ thống Microservices/Web-Mobile**, kết hợp thông qua **API Gateway hoặc ESB**.

---

## 1. Tóm tắt quyết định kiến trúc

| Thành phần hệ thống | Công nghệ đề xuất | Lý do chính |
|---|---|---|
| **Legacy Core Banking** | **SOAP** | Cần bảo mật cao, ràng buộc dữ liệu nghiêm ngặt, hỗ trợ giao dịch phân tán và đảm bảo toàn vẹn dữ liệu |
| **Microservices mới** | **REST** | Nhẹ, nhanh, dễ mở rộng, phù hợp Web/Mobile và phát triển tính năng nhanh |
| **Lớp tích hợp** | **API Gateway / ESB** | Chuyển đổi REST/JSON từ client thành SOAP/XML để giao tiếp với Core Banking |

---

## 2. Phân tích chi tiết: Vì sao chọn SOAP cho Legacy và REST cho Microservices?

### 2.1. Hiệu suất — Performance

#### REST cho Microservices

REST có lợi thế lớn về hiệu suất vì thường sử dụng **JSON**, một định dạng dữ liệu gọn nhẹ hơn XML.

**Ưu điểm nổi bật:**

- Payload nhỏ hơn.
- Tốn ít tài nguyên CPU hơn khi parse dữ liệu.
- Phản hồi nhanh hơn.
- Phù hợp với thiết bị di động, nơi băng thông và pin bị giới hạn.
- Dễ đáp ứng lưu lượng truy cập lớn từ Web/Mobile.

**Kết luận:** REST phù hợp với các dịch vụ mới cần tốc độ cao và trải nghiệm người dùng tốt.

#### SOAP cho Legacy

SOAP sử dụng **XML**, nên gói tin thường nặng hơn và tốn nhiều tài nguyên xử lý hơn. Tuy nhiên, trong Core Banking, tốc độ không phải yếu tố duy nhất.

**Lý do vẫn phù hợp:**

- Core Banking ưu tiên **độ chính xác** và **toàn vẹn dữ liệu** hơn tốc độ vài mili-giây.
- Hệ thống nội bộ thường có băng thông và tài nguyên server đủ mạnh.
- SOAP phù hợp với các nghiệp vụ tài chính quan trọng cần kiểm soát chặt chẽ.

**Kết luận:** SOAP có thể chậm hơn REST, nhưng đổi lại có tính ổn định và an toàn cao hơn trong hệ thống lõi.

---

### 2.2. Bảo mật — Security

#### SOAP cho Legacy

SOAP mạnh ở khả năng bảo mật cấp độ thông điệp thông qua **WS-Security**.

**Điểm mạnh:**

- Hỗ trợ mã hóa thông điệp.
- Hỗ trợ chữ ký điện tử.
- Hỗ trợ chống chối bỏ trách nhiệm.
- Bảo vệ dữ liệu ngay cả khi đi qua nhiều hệ thống trung gian.
- Phù hợp với các quy định nghiêm ngặt trong lĩnh vực tài chính.

**Ví dụ:** Một giao dịch chuyển tiền có thể cần chữ ký số, mã hóa đầu cuối và kiểm tra tính toàn vẹn dữ liệu qua nhiều bước xử lý.

**Kết luận:** SOAP phù hợp với giao dịch tài chính lõi, nơi bảo mật phải ở mức rất cao.

#### REST cho Microservices

REST thường bảo mật bằng **HTTPS/TLS**, kết hợp với **JWT** hoặc **OAuth2**.

**Phù hợp cho:**

- Đăng nhập người dùng.
- Xem số dư.
- Xem lịch sử giao dịch.
- Gọi API từ Web/Mobile.
- Các thao tác không yêu cầu tiêu chuẩn bảo mật phức tạp như WS-Security.

**Kết luận:** REST đủ an toàn cho lớp ứng dụng bên ngoài, nhưng không chặt chẽ bằng SOAP trong các giao dịch tài chính lõi.

---

### 2.3. Độ phức tạp — Complexity

#### REST cho Microservices

REST đơn giản hơn trong thiết kế và triển khai.

**Ưu điểm:**

- Không cần WSDL.
- Dễ test bằng Postman, Swagger hoặc trình duyệt.
- Dễ học, dễ triển khai.
- Phù hợp với nhiều ngôn ngữ lập trình hiện đại.
- Giúp rút ngắn thời gian phát triển sản phẩm.

**Kết luận:** REST giúp đội phát triển Microservices làm việc nhanh và linh hoạt hơn.

#### SOAP cho Legacy

SOAP phức tạp hơn vì yêu cầu hợp đồng dịch vụ rõ ràng thông qua **WSDL**.

**Đặc điểm:**

- Client và server phải thống nhất cấu trúc dữ liệu trước khi giao tiếp.
- WSDL giúp hạn chế lỗi sai kiểu dữ liệu.
- Phù hợp với các giao dịch có yêu cầu nghiêm ngặt.
- Hỗ trợ các chuẩn doanh nghiệp như WS-AtomicTransaction.

**Kết luận:** SOAP khó phát triển hơn ban đầu, nhưng sự chặt chẽ này giúp giảm lỗi trong hệ thống tài chính lớn.

---

### 2.4. Khả năng mở rộng và tốc độ phát triển
#### Scalability & Development Speed

#### REST cho Microservices

REST thường được thiết kế theo nguyên tắc **Stateless**.

**Lợi ích của Stateless:**

- Mỗi request độc lập với nhau.
- Server không cần lưu trạng thái phiên làm việc của client.
- Dễ scale out bằng cách thêm nhiều server.
- Phù hợp với hệ thống có lượng người dùng tăng đột biến.

**Kết luận:** REST rất phù hợp cho các dịch vụ mới cần mở rộng nhanh.

#### SOAP cho Legacy

SOAP thường có tính liên kết chặt hơn.

**Hạn chế:**

- Thay đổi WSDL có thể ảnh hưởng đến nhiều client.
- Khó thay đổi nhanh như REST.
- Ít linh hoạt hơn trong phát triển tính năng mới.

**Tuy nhiên:** Core Banking thường là hệ thống ổn định, ít thay đổi, nên nhược điểm này có thể chấp nhận được.

**Kết luận:** SOAP phù hợp với hệ thống lõi ổn định, còn REST phù hợp với hệ thống mới cần thay đổi nhanh.

---

### 2.5. Khả năng tích hợp — Integration

#### REST cho Microservices

REST được hỗ trợ rộng rãi trong hầu hết các nền tảng hiện đại.

**Phù hợp với:**

- Web app.
- Mobile app.
- JavaScript, Java, Python, Go, Node.js.
- Hệ thống Frontend hiện đại.
- Các dịch vụ public API.

**Kết luận:** REST là lựa chọn tự nhiên cho các hệ thống mới và ứng dụng client hiện đại.

#### SOAP cho Legacy

SOAP tích hợp tốt với các nền tảng doanh nghiệp truyền thống.

**Phù hợp với:**

- Java EE.
- .NET.
- ERP.
- CRM.
- Các hệ thống ngân hàng cũ.
- Môi trường cần tự động sinh code từ WSDL.

**Kết luận:** SOAP vẫn có giá trị lớn trong môi trường doanh nghiệp cũ, đặc biệt là tài chính và ngân hàng.

---

## 3. Báo cáo phân tích kiến trúc

### 3.1. Hệ thống cũ — Legacy Core Banking

**Lựa chọn đề xuất:** SOAP

#### Lý do 1: Đảm bảo an toàn giao dịch tuyệt đối

Các giao dịch cốt lõi của ngân hàng cần đảm bảo tính toàn vẹn dữ liệu ở mức rất cao.

SOAP hỗ trợ **WS-AtomicTransaction**, giúp điều phối giao dịch phân tán. Nếu một bước trong chuỗi giao dịch bị lỗi, toàn bộ giao dịch có thể được rollback.

**Ví dụ:**

```text
Bước 1: Trừ tiền tài khoản A
Bước 2: Cộng tiền tài khoản B
Bước 3: Ghi log giao dịch
Bước 4: Gửi xác nhận
```

Nếu bước 2 thất bại, hệ thống phải rollback bước 1 để tránh mất tiền hoặc sai lệch dữ liệu.

#### Lý do 2: Bảo mật cấp độ thông điệp

SOAP hỗ trợ **WS-Security**, cho phép mã hóa và ký số trực tiếp trên thông điệp.

Điều này đặc biệt quan trọng khi dữ liệu phải đi qua:

- Proxy.
- Middleware.
- Hệ thống bên thứ ba.
- Nhiều service nội bộ.
- Các điểm trung gian trong doanh nghiệp.

#### Lý do 3: Ràng buộc dữ liệu nghiêm ngặt

WSDL đóng vai trò như một bản hợp đồng giữa client và server.

**Lợi ích:**

- Định nghĩa rõ input/output.
- Giảm lỗi sai kiểu dữ liệu.
- Giúp hệ thống giao tiếp ổn định.
- Phù hợp với nghiệp vụ tài chính có yêu cầu chính xác cao.

---

### 3.2. Hệ thống mới — Microservices

**Lựa chọn đề xuất:** REST

#### Lý do 1: Tối ưu trải nghiệm người dùng

REST sử dụng JSON nên dữ liệu truyền tải nhẹ, nhanh và phù hợp với Web/Mobile.

**Lợi ích:**

- Giảm độ trễ.
- Tiết kiệm băng thông.
- Dễ hiển thị dữ liệu trên frontend.
- Phù hợp với ứng dụng có nhiều người dùng.

#### Lý do 2: Dễ mở rộng

REST Stateless giúp hệ thống dễ mở rộng theo chiều ngang.

Khi lượng người dùng tăng, doanh nghiệp có thể thêm nhiều server chạy cùng service mà không phụ thuộc vào session lưu trên một server cụ thể.

#### Lý do 3: Tăng tốc độ phát triển

REST đơn giản, phổ biến và dễ tích hợp.

Đội phát triển có thể nhanh chóng:

- Tạo API mới.
- Test API.
- Tích hợp với frontend.
- Tách module thành service độc lập.
- Triển khai tính năng mới nhanh hơn.

---

## 4. Khuyến nghị chiến lược cho công ty

### 4.1. Áp dụng mô hình Hybrid

Không nên ép toàn bộ hệ thống dùng một giao thức duy nhất.

**Chiến lược hợp lý:**

| Khu vực | Công nghệ |
|---|---|
| Backend lõi nội bộ | SOAP |
| Web/Mobile API | REST |
| Lớp chuyển đổi | API Gateway hoặc ESB |

**Mô hình đề xuất:**

```text
Mobile/Web Client
       |
       | REST / JSON
       v
API Gateway / ESB
       |
       | SOAP / XML
       v
Legacy Core Banking
```

---

### 4.2. Triển khai API Gateway hoặc ESB

API Gateway hoặc ESB đóng vai trò như lớp trung gian chuyển đổi giữa hệ thống mới và hệ thống cũ.

#### Nhiệm vụ chính

- Nhận request REST/JSON từ Web/Mobile.
- Chuyển đổi dữ liệu sang SOAP/XML.
- Gọi hệ thống Legacy Core Banking.
- Nhận kết quả SOAP/XML.
- Parse ngược về JSON.
- Trả response cho client.

#### Lợi ích

- Client không cần biết hệ thống lõi dùng SOAP.
- Hạn chế thay đổi trực tiếp vào Core Banking.
- Dễ kiểm soát bảo mật, logging và monitoring.
- Tạo lớp bảo vệ giữa hệ thống mới và hệ thống cũ.

---

### 4.3. Chuyển đổi cuốn chiếu bằng Strangler Fig Pattern

Không nên thay thế toàn bộ hệ thống Legacy ngay lập tức vì rủi ro rất cao.

**Cách làm an toàn hơn:**

1. Giữ nguyên các chức năng Core Banking đang ổn định.
2. Xây dựng các module mới bằng Microservices REST.
3. Tách dần các nghiệp vụ ít rủi ro ra khỏi hệ thống cũ.
4. Khi module mới ổn định, giảm dần phụ thuộc vào Legacy.

**Các module phù hợp để tách trước:**

- Ví điện tử.
- Quản lý ưu đãi.
- Tích điểm khách hàng.
- Thông báo giao dịch.
- Lịch sử hoạt động người dùng.
- Quản lý chiến dịch marketing.

---

## 5. Bảng so sánh tổng hợp SOAP và REST

| Tiêu chí | SOAP | REST |
|---|---|---|
| Bản chất | Giao thức | Kiểu kiến trúc |
| Định dạng dữ liệu | XML | Thường là JSON |
| Hiệu suất | Nặng hơn | Nhẹ hơn, nhanh hơn |
| Bảo mật | WS-Security, message-level security | HTTPS/TLS, JWT, OAuth2 |
| Contract | Chặt chẽ qua WSDL | Linh hoạt hơn |
| Giao dịch phân tán | Hỗ trợ tốt qua WS-AtomicTransaction | Không phải điểm mạnh |
| Độ phức tạp | Cao | Thấp |
| Khả năng mở rộng | Thấp hơn REST | Rất tốt nhờ Stateless |
| Phù hợp với | Core Banking, ERP, hệ thống doanh nghiệp cũ | Web, Mobile, Microservices |
| Tốc độ phát triển | Chậm hơn | Nhanh hơn |

---

## 6. Kết luận

Đối với hệ thống tài chính, lựa chọn tốt nhất không phải là chỉ dùng SOAP hoặc chỉ dùng REST, mà là **kết hợp cả hai theo đúng vai trò**.

- **SOAP** nên được giữ lại cho hệ thống **Legacy Core Banking**, nơi yêu cầu bảo mật, tính toàn vẹn dữ liệu và giao dịch phân tán rất cao.
- **REST** nên được sử dụng cho các **Microservices mới**, đặc biệt là các API phục vụ Web/Mobile, nơi cần tốc độ, tính linh hoạt và khả năng mở rộng.
- **API Gateway hoặc ESB** nên được dùng làm lớp trung gian để kết nối hai thế giới này.

> **Khuyến nghị cuối cùng:**  
> Doanh nghiệp nên áp dụng kiến trúc **Hybrid: SOAP bên trong, REST bên ngoài**, kết hợp với chiến lược **Strangler Fig Pattern** để hiện đại hóa hệ thống một cách an toàn, từng bước và ít rủi ro.

---

## 7. Glossary — Thuật ngữ cốt lõi

### SOAP — Simple Object Access Protocol

SOAP là giao thức truyền tải dữ liệu dựa trên XML, có quy tắc nghiêm ngặt và thường được dùng trong các hệ thống doanh nghiệp cần độ tin cậy cao.

### REST — Representational State Transfer

REST là kiểu kiến trúc thiết kế API dựa trên tài nguyên. REST thường sử dụng các phương thức HTTP như GET, POST, PUT và DELETE để thao tác dữ liệu.

### WSDL — Web Services Description Language

WSDL là file XML mô tả chi tiết cách một SOAP Web Service hoạt động, bao gồm các hàm, dữ liệu đầu vào, dữ liệu đầu ra và quy tắc giao tiếp.

### JSON — JavaScript Object Notation

JSON là định dạng dữ liệu dạng key-value, gọn nhẹ, dễ đọc và phổ biến trong REST API.

### XML — eXtensible Markup Language

XML là ngôn ngữ đánh dấu dùng các thẻ để mô tả dữ liệu. SOAP sử dụng XML làm định dạng dữ liệu chính.

### WS-Security

WS-Security là tiêu chuẩn bảo mật mở rộng của SOAP, hỗ trợ mã hóa thông điệp, chữ ký số và bảo vệ dữ liệu ở cấp độ message.

### Stateless — Phi trạng thái

Stateless nghĩa là server không lưu trạng thái của client giữa các request. Mỗi request phải chứa đầy đủ thông tin cần thiết để server xử lý độc lập.

### API Gateway

API Gateway là lớp trung gian tiếp nhận request từ client, định tuyến đến service phù hợp, xử lý xác thực, logging, monitoring và có thể chuyển đổi dữ liệu giữa REST và SOAP.

### ESB — Enterprise Service Bus

ESB là hệ thống trung gian tích hợp trong doanh nghiệp, giúp kết nối nhiều hệ thống khác nhau, đặc biệt là các hệ thống Legacy.

### Strangler Fig Pattern

Strangler Fig Pattern là chiến lược thay thế hệ thống cũ từng phần. Thay vì viết lại toàn bộ hệ thống ngay lập tức, doanh nghiệp sẽ tách dần các chức năng sang hệ thống mới.
