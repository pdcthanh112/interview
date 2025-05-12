

[1. Bean](#1-bean)  
[2. Tại sao nên dùng constructor inject thay vì @Autowired](#1-rabbitmq)


### 1. Bean

Bean là những module chính của chương trình, được quản lý và cấu hình bởi Spring IoC Container. Các bean trong Spring Boot được quản lý bởi ApplicationContext (là một loại IoC Container).

Để Spring biết được đâu là bean thì cần phải đánh dấu `@Component`, `@Service`, `@Controller`,..v..v….

Bean trong Spring Boot thường là các đối tượng có thể được tái sử dụng, và chúng có thể được tiếp cận từ bất kỳ đâu trong ứng dụng.

**Quá trình khởi tạo và vòng đời:**

Spring Boot tạo bean instance thông qua quá trình quản lý bean của mình, dựa trên các annotations và cấu hình cung cấp. Các bước chính để Spring Boot tạo bean instance là:

1. **Quét (Scanning)**: Spring Boot quét toàn bộ project của bạn để tìm các lớp được đánh dấu bằng các annotations như `@Component`, `@Service`, `@Repository`, `@Controller`, v.v...
2. **Đăng ký (Registration)**: Khi tìm thấy các lớp được đánh dấu, Spring Boot đăng ký các lớp đó vào ApplicationContext như các bean. Điều này cho phép bạn có thể tiếp cận và sử dụng các bean này từ bất kỳ đâu trong ứng dụng.
3. **Khởi tạo (Initialization)**: Sau khi đăng ký, Spring Boot khởi tạo các bean bằng cách sử dụng các constructor, setter method, hoặc factory method đã cung cấp.
4. **Cấu hình (Configuration)**: có thể cấu hình các bean bằng cách sử dụng các annotations như `@Autowired` để tự động inject các phụ thuộc (dependencies) vào bean, `@Value` để inject giá trị từ file properties, hoặc các annotations khác để chỉ định phương thức cấu hình.
5. **Quản lý vòng đời (Lifecycle Management)**: Spring Boot quản lý vòng đời của các bean, bao gồm việc khởi tạo, inject phụ thuộc, gọi các phương thức khởi động và hủy các bean khi cần thiết.

### Tại sao nên dùng constructor inject thay vì @Autowired