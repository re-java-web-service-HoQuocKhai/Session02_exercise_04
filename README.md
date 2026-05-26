PHẦN 1:
Phân tích chi tiết: Tại sao chọn SOAP cho Legacy và REST cho Microservices?
1. Hiệu suất (Performance)
   REST (Cho Microservices): Ưu điểm lớn nhất của REST là sự mỏng nhẹ. Nó thường sử dụng định dạng JSON, có kích thước dữ liệu (payload) nhỏ hơn rất nhiều so với XML và tốn ít CPU để phân tích cú pháp (parse). Điều này mang lại tốc độ phản hồi nhanh, cực kỳ tối ưu cho các thiết bị di động (nơi băng thông mạng và pin bị giới hạn) và đáp ứng được lưu lượng truy cập lớn của hệ thống mới.

SOAP (Cho Legacy): Mặc dù SOAP nặng nề hơn (vì chỉ dùng định dạng XML với nhiều thẻ tag mô tả chi tiết), điều này có thể chấp nhận được trong hệ thống ngân hàng cốt lõi (Core Banking). Ở hệ thống này, tính chính xác và toàn vẹn của dữ liệu được đặt lên trên tốc độ tính bằng mili-giây. Các server xử lý nghiệp vụ nội bộ thường có sẵn băng thông rộng và tài nguyên tính toán mạnh mẽ để xử lý các gói tin SOAP.

2. Bảo mật (Security)
   SOAP (Cho Legacy): Đây là "vũ khí tối thượng" của SOAP. Nó tích hợp sẵn bộ tiêu chuẩn WS-Security ở cấp độ thông điệp (message-level). WS-Security cung cấp mã hóa đầu cuối phức tạp, chữ ký điện tử (non-repudiation - chống chối bỏ trách nhiệm) qua nhiều khâu trung gian, điều mà các quy định tài chính quốc tế bắt buộc phải có cho các giao dịch chuyển tiền cốt lõi.

REST (Cho Microservices): REST thường phụ thuộc vào các giao thức bảo mật lớp truyền tải (như HTTPS/TLS) kết hợp với các cơ chế xác thực hiện đại như JWT (JSON Web Tokens) hoặc OAuth2. Nó hoàn toàn đủ an toàn cho việc xác thực người dùng trên app/web, xem số dư hoặc lịch sử giao dịch, nhưng thiếu các tiêu chuẩn ràng buộc chặt chẽ bằng văn bản (contract) như WS-Security.

3. Độ phức tạp (Complexity)
   REST (Cho Microservices): Rất dễ tiếp cận, thiết kế và triển khai. Lập trình viên không cần các công cụ cấu hình phức tạp hay các tệp mô tả (như WSDL). Tính đơn giản này giúp đội ngũ phát triển Microservices rút ngắn thời gian đưa sản phẩm ra thị trường (Time-to-Market).

SOAP (Cho Legacy): SOAP rất phức tạp. Nó đòi hỏi một bản hợp đồng nghiêm ngặt (WSDL - Web Services Description Language) để cả client và server thống nhất cấu trúc dữ liệu trước khi giao tiếp. Dù khó phát triển ban đầu, sự chặt chẽ này giúp ngăn ngừa tối đa các lỗi sai lệch kiểu dữ liệu (data type mismatch) trong các giao dịch tài chính lớn. Thêm vào đó, SOAP hỗ trợ WS-AtomicTransaction, giải quyết hoàn hảo bài toán giao dịch phân tán (Distributed Transactions) để đảm bảo tính ACID (nếu một giao dịch lỗi, toàn bộ chuỗi hệ thống sẽ tự động rollback).

4. Khả năng phát triển (Scalability & Development Speed)
   REST (Cho Microservices): Kiến trúc cốt lõi của REST là Stateless (Phi trạng thái - server không lưu trữ trạng thái của client giữa các request). Điều này giúp hệ thống cực kỳ dễ dàng mở rộng theo chiều ngang (Scale out - thêm máy chủ mới) khi lượng người dùng tăng đột biến. Việc thay đổi tính năng cũng linh hoạt và ít gây vỡ hệ thống.

SOAP (Cho Legacy): SOAP có tính liên kết chặt (tight coupling). Bất kỳ thay đổi nhỏ nào trong cấu trúc (WSDL) cũng có thể làm "gãy" kết nối của toàn bộ client đang sử dụng. Tuy nhiên, Core Banking là hệ thống ít khi thay đổi tính năng (chủ yếu là duy trì tính ổn định), nên nhược điểm này không phải là vấn đề quá lớn.

5. Khả năng tích hợp (Integration)
   REST (Cho Microservices): Được hỗ trợ mặc định bởi hầu hết mọi ngôn ngữ lập trình hiện đại (JavaScript, Python, Go, Java...) và là "ngôn ngữ mẹ đẻ" của các trình duyệt web và ứng dụng iOS/Android.

SOAP (Cho Legacy): Tích hợp cực tốt với các nền tảng doanh nghiệp truyền thống (như Java EE, .NET, hoặc các hệ thống ERP, CRM cũ). Các công cụ của môi trường doanh nghiệp có thể tự động sinh ra mã nguồn (auto-generate code) từ file WSDL một cách hoàn hảo, tiết kiệm công sức tích hợp nội bộ giữa các dịch vụ Core Banking với nhau.

BÁO CÁO PHÂN TÍCH KIẾN TRÚC: LỰA CHỌN WEB SERVICE CHO HỆ THỐNG TÀI CHÍNH
1. Giải thích lý do cụ thể cho từng quyết định kiến trúc
   Hệ thống cũ (Legacy Core Banking) — Lựa chọn: SOAP

Đảm bảo an toàn giao dịch tối tuyệt đối: Các giao dịch cốt lõi của ngân hàng yêu cầu tính toàn vẹn dữ liệu (ACID) ở mức khắt khe nhất. SOAP hỗ trợ tích hợp sẵn tiêu chuẩn WS-AtomicTransaction, cho phép kiểm soát và điều phối các giao dịch phân tán phức tạp, tự động rollback (hoàn tác) toàn bộ chuỗi hệ thống nếu có bất kỳ mắt xích nào xảy ra lỗi.

Bảo mật toàn diện cấp độ thông điệp (Message-level Security): Chuẩn WS-Security giúp mã hóa dữ liệu ngay từ gốc (phía gửi) và chỉ giải mã ở đích đến cuối cùng (phía nhận). Điều này giúp thông tin tài chính luôn được bảo vệ an toàn tuyệt đối kể cả khi phải truyền qua các máy chủ trung gian, proxy hoặc hệ thống của bên thứ ba.

Ràng buộc dữ liệu nghiêm ngặt: Tệp mô tả WSDL đóng vai trò như một bản hợp đồng pháp lý bằng mã nguồn giữa Client và Server. Mọi kiểu dữ liệu bắt buộc phải khớp 100%, giúp loại bỏ hoàn toàn các lỗi sai lệch định dạng dữ liệu trong các phép tính toán tài chính lớn.

Hệ thống mới (Microservices) — Lựa chọn: REST

Tối ưu hóa trải nghiệm người dùng cuối (Hiệu suất): REST sử dụng cấu trúc JSON rất mỏng nhẹ, giúp giảm thiểu dung lượng gói tin truyền tải qua mạng. Việc này giúp tiết kiệm tối đa băng thông và điện năng trên các thiết bị di động, mang lại tốc độ phản hồi cực nhanh cho ứng dụng Web/Mobile.

Khả năng mở rộng không giới hạn (Scalability): Với tính chất Stateless (Phi trạng thái), mỗi request gửi lên REST API hoàn toàn độc lập và không phụ thuộc vào trạng thái trước đó. Nhờ vậy, hệ thống có thể dễ dàng nhân bản và mở rộng thêm các máy chủ (Scale out) theo chiều ngang bất cứ khi nào lượng người dùng ứng dụng tăng đột biến.

Tăng tốc độ phát triển và triển khai (Time-to-Market): Kiến trúc REST đơn giản, dễ tiếp cận và được hỗ trợ mặc định bởi toàn bộ các ngôn ngữ lập trình hiện đại. Đội ngũ phát triển Microservices có thể nhanh chóng xây dựng, thử nghiệm và tung ra các tính năng mới mà không bị cản trở bởi các cấu hình nặng nề.

2. Khuyến nghị chiến lược cho công ty
   Áp dụng mô hình kết hợp Hybrid (SOAP nội bộ & REST ngoại vi): Không cố gắng đồng nhất một giao thức duy nhất cho toàn doanh nghiệp. Hãy giữ lại nền tảng SOAP vững chắc để xử lý luồng nghiệp vụ lõi (Back-end) nội bộ của Core Banking và triển khai REST làm giao diện chính tiếp xúc với các ứng dụng phía Client (Front-end).

Triển khai lớp trung gian chuyển đổi (API Gateway / ESB): Xây dựng một lớp API Gateway đóng vai trò làm "thông dịch viên". Khi ứng dụng Mobile/Web gửi yêu cầu dạng REST/JSON, API Gateway sẽ tiếp nhận, chuyển đổi cấu trúc (mapping) thành gói tin SOAP/XML để giao tiếp với hệ thống Legacy core banking, sau đó parse ngược kết quả từ SOAP/XML thành JSON để trả về cho Client.

Chiến lược chuyển đổi cuốn chiếu (Strangler Fig Pattern): Đối với hệ thống cũ, giữ nguyên các tính năng đang chạy ổn định qua các cổng SOAP. Khi phát triển các module nghiệp vụ mới (như ví điện tử, quản lý ưu đãi, tích điểm), hãy tách chúng ra thành các Microservices độc lập chạy REST hoàn toàn để giảm tải dần cho hệ thống Legacy.

3. Thuật ngữ cốt lõi (Glossary - Tổng hợp từ Lesson 02)
   SOAP (Simple Object Access Protocol): Giao thức truyền tải dữ liệu dựa trên nền tảng XML, tuân thủ các quy tắc thiết kế nghiêm ngặt và hoạt động theo mô hình hướng giao thức (protocol-based).

REST (Representational State Transfer): Kiểu kiến trúc hệ thống (architectural style) hoạt động dựa trên các tài nguyên (Resources) và tận dụng trực tiếp các phương thức của HTTP (GET, POST, PUT, DELETE) để thao tác dữ liệu.

WSDL (Web Services Description Language): File ngôn ngữ XML dùng để mô tả chi tiết cách thức hoạt động, các hàm cung cấp, cấu trúc dữ liệu đầu vào/đầu ra của một SOAP Web Service, đóng vai trò như hợp đồng ràng buộc giữa các hệ thống.

JSON (JavaScript Object Notation): Định dạng trao đổi dữ liệu văn bản mỏng nhẹ, có cấu trúc dạng key-value, dễ đọc viết đối với con người và là tiêu chuẩn truyền tải dữ liệu phổ biến nhất trong REST API.

XML (eXtensible Markup Language): Ngôn ngữ đánh dấu mở rộng sử dụng các thẻ tự định nghĩa để cấu trúc dữ liệu, là định dạng dữ liệu bắt buộc và duy nhất mà giao thức SOAP chấp nhận.

WS-Security (Web Services Security): Tiêu chuẩn mở rộng cấp cao của SOAP nhằm áp dụng các cơ chế bảo mật nâng cao trực tiếp vào gói tin như ký số (digital signature) và mã hóa thông điệp.

Stateless (Phi trạng thái): Nguyên tắc thiết kế của REST, quy định rằng mỗi yêu cầu từ Client gửi lên Server phải chứa toàn bộ thông tin cần thiết để xử lý độc lập. Server không lưu giữ bất kỳ ngữ cảnh hoặc phiên làm việc (session) nào của Client trong bộ nhớ.


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
