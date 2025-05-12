## Table of content

[1. Nguyen ly SOLID](#1-nguyen-ly-solid)  
[2. Định lý CAP](#2-dinh-ly-cap)  
[3. IoC (Inversion of Control)](#3-ioc-inversion-of-control)


### 1. Nguyen ly SOLID
- S: Single Responsibility Principle (SRP)
    - Mỗi lớp (class) trong app chỉ nên có một trách nhiệm duy nhất.
    Hay nói cách khác, một lớp chỉ nên có một lý do duy nhất để thay đổi.
- O: Open/Closed Principle (OCP)
    - Open for extend but close for modifying. Nguyên lý này đề cập đến việc có thể thoải mái mở rộng 1 class/function nhưng không được phép chỉnh sửa trực tiếp trên class đó (Tất nhiên ở đây là nói đến việc mở rộng, phát triển tính năng chứ không phải là sửa lỗi)
- L: Liskov Substitution Principle (LSP)
    - Các đối tượng của một lớp con phải có thể thay thế cho các đối tượng của lớp cha mà không làm thay đổi tính đúng đắn của chương trình.
- I: Interface Segregation Principle (ISP)
    - Thay vì tạo ra 1 interface chung với tất cả các chức năng, thì nên tách ra thành nhiều interface con với từng nhóm chức năng cụ thể, điều này giảm sự implement dư thừa nếu 1 class phải implement 1 interface quá lớn
- D: Dependency Inversion Principle (DIP)
    - Các module cấp cao không nên phụ thuộc vào các modules cấp thấp. Cả 2 nên phụ thuộc vào abstraction. Interface (abstraction) không nên phụ thuộc vào chi tiết, mà ngược lại. ( Các class giao tiếp với nhau thông qua interface, không phải thông qua implementation)
    - Giả sử như ở Controller cần gọi Service, thì ở đây không nên inject trực tiếp class Service vào Controller mà nên tạo ra 1 Service Interface để Controller inject
    - Lợi ích là giúp giảm sự phục thuộc trực tiếp giữa Service và Controller  (loose coupling), và giúp dễ dàng mở rộng hơn (giả sử sau này muốn chỉnh sửa thì chỉ cần chỉnh sửa trong lớp Service Implement, hoặc muốn nâng cấp version thì tạo thêm 1 implement khác)
    - Khi chương trình được tạo, nó sẽ quét qua tất cả các lớp có gắn anotation là @Component, @Service, @Controller, @Repository, v.v…(tóm lại là các Bean), và đăng ký nó với Spring Context/IoC Container
    - Khi quét tới class Service Implement, vì class này có đánh anotation @Service nên nó sẽ được khai báo với IoC Container, với  type là chính nó, cùng với type là interface Service, vì nó là implementation của interface Service
    - Nên khi trong Controller inject Service, lúc đó Spring sẽ kiểm tra trong IoC Container xem có Bean nào có type là Service không, và nó tìm thấy class Service Implement (vì nó đã được gắn @Service), nên thật chất là Controller inject class Service Implement
    - Nếu 1 Service interface có 2 implement trở lên, ứng dụng sẽ báo lỗi, và ta phải đặt @Primary hoặc @Qualifier cho implement chính
  
    **Tại sao là interface mà không phải là abstract class:**
  - Interface hỗ trợ đa kế thừa, còn abstract class thì không

    ```java
    public interface Searchable {}
    public interface Sortable {}

    public class ProductServiceImpl implements ProductService, Searchable, Sortable {}
    ```

  - Trong interface chỉ có các abstract method, không có implementation → đảm bảo được tính thống nhất giữa các class implementation


### 2. Định lý CAP
- **C: Consistency**
    
    Tính nhất quán đảm bảo rằng mọi node trong hệ thống đều có cùng một trạng thái dữ liệu tại cùng một thời điểm. Khi bạn cập nhật dữ liệu trên một node (ví dụ: node A), tất cả các node khác (B, C...) phải ngay lập tức phản ánh giá trị đã được cập nhật. Nếu không thể đảm bảo điều này, hệ thống sẽ từ chối yêu cầu thay vì trả về dữ liệu cũ.
    
    Đây là lựa chọn của các lĩnh vực quan trọng như ngân hàng, thanh toán, hay đặt vé máy bay, nơi mà bất kỳ sai sót nào cũng có thể dẫn đến hậu quả nghiêm trọng.
    
- **A: Availability**
    
    Tính sẵn sàng đảm bảo rằng hệ thống luôn phản hồi các yêu cầu của người dùng, miễn là node phục vụ yêu cầu đó không bị lỗi. Mọi yêu cầu gửi đến một node hoạt động bình thường đều phải được phản hồi. Không có tình trạng "server đang bận, vui lòng thử lại sau". Nếu phải lựa chọn giữa việc trả về dữ liệu cũ hoặc không trả về gì, hệ thống sẽ ưu tiên trả về dữ liệu cũ.
    
    Đây là lựa chọn của các trang mạng xã hội, blog hay các trang tin tức, nơi mà người dùng chấp nhận việc nhìn thấy nội dung cũ còn hơn không thấy bất kỳ thông tin nào.
    
- **P: Partition Tolerance**
    
    Partition Tolerance đảm bảo rằng hệ thống vẫn tiếp tục hoạt động ngay cả khi xảy ra sự cố mạng khiến các node trong hệ thống không thể kết nối với nhau. Partition Tolerance không phải là một lựa chọn, bạn bắt buộc phải thiết kế hệ thống có khả năng chịu được sự cố này, bởi vì các vấn đề về mạng như mất kết nối hay chập chờn là điều không thể tránh khỏi trong thực tế.

    
Trong thực tế, trong hệ thống không thể đảm bảo thoã mãn cả 3 tính chất của CAP, mà bắt buộc phải có sự đánh đổi

***Hệ thống ưu tiên CP** (Consistency và Partition Tolerance): sẽ đặt tính đúng đắn của dữ liệu lên trên tất cả, thậm chí là hơn cả sự sẵn sàng phục vụ của hệ thống. Điều này đồng nghĩa với việc, khi xảy ra Partition, hệ thống sẽ từ chối các yêu cầu nếu không thể đảm bảo tính nhất quán giữa tất cả các node. 

Ví dụ: transaction chuyển tiền.

- User: Chuyển tiền 1.000.000 VND đến tài khoản B.
- System: "Xin lỗi, không thể xác nhận giao dịch lúc này do sự cố kết nối. Vui lòng thử lại sau."
- Hệ thống CP từ chối xử lý giao dịch này bởi vì nó không thể đảm bảo dữ liệu về số dư tài khoản sẽ được cập nhật đồng bộ trên tất cả các node, một số node trong hệ thống đang không phản hồi, và điều gì sẽ xảy ra nếu người dùng chuyển quá số tiền họ có trong tài khoản? Hay người gửi nói rằng giao dịch thành công, nhưng người nhận không nhận được tiền.

***Hệ thống ưu tiên AP** (Availability và Partition Tolerance) sẽ đặt mục tiêu duy trì tính sẵn sàng lên hàng đầu, ngay cả khi điều này dẫn đến việc dữ liệu tạm thời không nhất quán giữa các node. Trong trường hợp xảy ra Partition, mỗi node trong cluster sẽ tiếp tục xử lý yêu cầu của người dùng, bất kể chúng có thể tạm thời "không đồng ý" về trạng thái dữ liệu.

Ví dụ: trong 1 ứng dụng mạng xã hôi:

- User 1 (kết nối với Node A): Thêm bình luận: "abc xyz!"
- User 2 (kết nối với Node B): Không thấy bình luận đó
- System: "Không vấn đề gì! Họ sẽ thấy bình luận khi mạng được khôi phục và dữ liệu đồng bộ lại."

Hệ thống AP đánh đổi tính nhất quán tạm thời để đảm bảo rằng mỗi yêu cầu từ người dùng được xử lý ngay lập tức. Điều này đặc biệt phù hợp cho các ứng dụng ưu tiên trải nghiệm liền mạch của người dùng – việc hệ thống "không khả dụng" (downtime) sẽ gây khó chịu nhiều hơn so với việc hiển thị dữ liệu chưa được cập nhật hoặc không đồng nhất.

### 3. IoC (Inversion of Control)
- Đảo ngược luồng kiểm soát
- Giả sử như ở Controller cần gọi đến Service để xử lý, thay vì ở Controller sẽ tạo Service service = new Service(), thì sẽ inject Service đã được tạo trước đó từ bên ngoài vào (Bằng cơ chế Dependency Injection, hay nói cách khác DI là 1 kỹ thuật để IoC)
- Lúc này các Bean (các Service, Repository, v.v..) sẽ do Framework (Srping Context/IoC Container) quản lý tập trung, chứ không phải do nơi khởi tạo quản lý
- Các Bean được inject sẽ được tự động implenent cơ chế Singleton
- IoC giúp dễ dàng kiểm soát, quản lý và test các Bean, tránh tình trạng tạo mới rải rác ở nhiều nơi dẫn đến khó hoặc sót khi quản lý và test
  
*Dùng Java thuần (không Spring boot): vẫn có thể DI (thông qua constructor), nhưng không thể IoC, do không có công cụ quản lý vòng đời của bean. Đối tượng phải được tạo mới bằng Service s = new Service() (và phải tự implement các cơ chế như singleton, v.v), và destroy khi không sử dụng nữa bằng cách ngắt tham chiếus = null (hoặc nếu service có tài nguyên cần đóng, ví dụ như kết nối file, database thì s.close()) và chờ GC thu gom