## Table of content

[1. SQL Injection](#1-sql-injection)   
[2. XSS (Cross-site Scripting)](#2-xss-cross-site-scripting)  
[3. CSRF (Cross-Site Request Forgery)](#3-cross---site-request-forgery)  
[4. Token-based Authentication và Session-based Authentication](#3-cross---site-request-forgery)  



### 1. SQL Injection
Lỗi bảo mật **SQL Injection** là một kỹ thuật tấn công trong lập trình web, bằng cách chèn các câu lệnh SQL độc hại vào các trường nhập liệu của ứng dụng. Khi ứng dụng không xử lý hoặc kiểm tra đầu vào một cách an toàn, các câu lệnh SQL này có thể được thực thi bởi cơ sở dữ liệu, dẫn đến các hậu quả nghiêm trọng như lộ thông tin, xóa dữ liệu, hoặc thậm chí kiểm soát cơ sở dữ liệu.

**Cách phòng ngừa SQL Injection**

1. **Sử dụng Prepared Statements (Câu lệnh chuẩn hóa)**
   
```typescript
let query = 'SELECT * FROM users WHERE username = ? AND password = ?';

db.execute(query, [username, password]);
```

**2. Sử dụng ORM (Object-Relational Mapping)**
    
Sử dụng các ORM như: Prisma, Sequelize, TypeORM có thể giúp giảm thiểu rủi ro SQL Injection, vì chúng tự động hóa các truy vấn SQL và tránh việc thao tác trực tiếp với câu lệnh SQL.
    
**3. Kiểm tra đầu vào (Input Validation)**

Luôn luôn kiểm tra và xử lý đầu vào từ người dùng trước khi sử dụng trong câu lệnh SQL. Đảm bảo rằng đầu vào không chứa các ký tự đặc biệt như `'`, `;`, `--`, hoặc các từ khoá SQL như `DROP`, `DELETE`, `UPDATE`, ...


### 2. XSS (Cross-site Scripting)

XSS (Cross-site Scripting) là một loại lỗ hổng bảo mật khi bị chèn mã độc (thường là Javascript) vào các ứng dụng website. Khi người dùng truy cập vào trang web bị nhiễm mã độc, trình duyệt sẽ tự động thực thi mã độc đó.

Các loại tấn công XSS phổ biến:

- **Reflected XSS:** Mã độc được chèn vào các URL, khi người dùng click vào URL đó thì mã độc sẽ được thực thi.
- **Stored XSS:** Mã độc được lưu trữ trên máy chủ, khi người dùng truy cập vào trang web sẽ kích hoạt mã độc.
- **DOM - based XSS:** Sử dụng các kỹ thuật JavaScript để thay đổi nội dung trang web và chèn mã độc vào DOM.

**Cách ngăn chặn tấn công XSS:**

- Validate input đầu vào (check length, format, regex, v.v..). Đối với các trường nội dung không validate kiểu đó được (vd như comment, feedback, v.v ) thì
 - Mã hoá ký tự đặc biệt (tag HTML):

| Ký tự | Mã HTML          |
|-------|------------------|
| &     | `&amp;`          |
| <     | `&lt;`           |
| >     | `&gt;`           |
| “     | `&quot;`         |
| ’     | `&#x27;`         |


- Validate dữ liệu bằng thư viện: DOMPurìy, npm sanitize-html
- Cấu hình CSP (CSP: là tập hợp danh sách các domain, các script, style, image, iframe mà trình duyệt được phép load trên website)


### 3. CSRF (Cross-Site Request Forgery)

**Tấn công CSRF** (Cross-Site Request Forgery) là một cuộc tấn công lợi dụng trạng thái đã xác thực của người dùng trên một trang web. Khi người dùng đăng nhập, trình duyệt của họ sẽ lưu một **session cookie**, và cookie này sẽ được gửi tự động mỗi khi người dùng gửi yêu cầu đến máy chủ của trang web đó. Nếu trang web chỉ dựa vào cookie để xác thực, kẻ tấn công có thể lừa người dùng thực hiện các hành động không mong muốn mà không cần họ phải nhập lại thông tin đăng nhập.

Cách ngăn chặn tấn công CSRF
- **Sử dụng CSRF Token**: Tạo và sử dụng CSRF token để xác thực mỗi yêu cầu từ người dùng có thực sự được gửi từ trang web hiện tại và không phải từ một trang web khác.
- **SameSite Cookies**: Đặt cờ SameSite cho các cookie để giảm khả năng bị tấn công CSRF.
- **Double Submit Cookie**: Thêm một cookie chứa giá trị CSRF token và so khớp với giá trị của token trong một trường ẩn trong mẫu yêu cầu.
- **Thiết lập CORS (Cross-Origin Resource Sharing)**: Điều chỉnh cài đặt CORS để hạn chế truy cập từ các trang web khác.
- **Logout CSRF Protection**: Đảm bảo rằng quá trình đăng xuất cũng được bảo vệ khỏi CSRF để ngăn chặn kẻ tấn công có thể tạo ra các liên kết hoặc yêu cầu giả.


### 4. Token-based Authentication và Session-based Authentication

**Token-based authentication** sử dụng các token để xác thực danh tính của người dùng sau khi họ đã đăng nhập thành công vào hệ thống. Các token thường được sử dụng bao gồm JWT (JSON Web Token) hoặc các token ngắn hơn như OAuth tokens. Cách hoạt động của token-based authentication như sau:

- **Đăng nhập**: Người dùng cung cấp thông tin đăng nhập (username/password) cho máy chủ.
- **Phản hồi**: Nếu thông tin đăng nhập hợp lệ, máy chủ sẽ tạo ra một token và gửi lại cho người dùng.
- **Lưu trữ token**: Người dùng lưu trữ token này (thường là trong local storage hoặc cookie) và gửi nó với mỗi yêu cầu tiếp theo đến server.
- **Xác thực**: Mỗi khi nhận được yêu cầu từ client kèm theo token, server kiểm tra tính hợp lệ của token để xác thực người dùng.

**Ưu điểm của token-based authentication:**

- **Stateless**: Không cần lưu trữ trạng thái xác thực trên server. Mỗi yêu cầu từ client đều chứa đủ thông tin cần thiết để xác thực.
- **Dễ dàng tích hợp**: Các ứng dụng client-side (ví dụ như SPA hoặc mobile apps) dễ dàng tích hợp và sử dụng các token.

**Nhược điểm của token-based authentication:**

- **Quản lý token**: Cần phải quản lý và bảo vệ token một cách an toàn để tránh bị đánh cắp.
- **Không thể hủy token**: Một khi token đã được phát hành, nó sẽ được xác thực cho đến khi hết hạn hoặc bị thu hồi.


**Session-based authentication** sử dụng một phiên làm việc (session) để duy trì trạng thái xác thực của người dùng trên server. Cách hoạt động của session-based authentication như sau:

- **Đăng nhập**: Người dùng cung cấp thông tin đăng nhập cho máy chủ.
- **Xác thực**: Máy chủ xác thực thông tin đăng nhập và tạo một phiên làm việc (session).
- **Lưu trữ session**: Server lưu trữ session này và gửi một cookie session ID cho client.
- **Xác thực tiếp theo**: Mỗi yêu cầu từ client kèm theo session ID, server kiểm tra session ID này có hợp lệ không để xác thực người dùng.

**Ưu điểm của session-based authentication:**

- **Quản lý dễ dàng**: Server có thể dễ dàng hủy session khi người dùng đăng xuất hoặc phiên làm việc hết hạn.
- **Bảo mật cao**: Session ID không lưu trên client-side, giảm nguy cơ bị đánh cắp.

**Nhược điểm của session-based authentication:**

- **Stateful**: Server cần phải duy trì trạng thái phiên làm việc cho mỗi người dùng, đôi khi cần lưu trữ trạng thái phiên làm việc trên nhiều server.
- **Khó tích hợp**: Khó tích hợp với các ứng dụng client-side (SPA) và yêu cầu sử dụng cookie để lưu trữ session ID.