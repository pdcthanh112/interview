# Câu hỏi phỏng vấn Node.js

![](./assets/nodejs.jpeg)

## Node.js là gì

Nodejs là một nền tảng được xây dựng, vận hành tại V8 JavaScript runtime của Chrome. Với Nodejs, bạn có thể chạy JavaScript trên server và thể xây dựng, phát triển các ứng dụng mạng nhanh chóng và dễ dàng.

Nền tảng này bắt đầu được xây dựng, phát triển tại California từ năm 2009 với phần core phía dưới được lập trình bằng C++ gần như 100%. Điều này tạo nên ưu thế về tốc độ xử lý cũng như hiệu năng của nền tảng này. Đến nay, Nodejs vẫn đang "gây bão" trong cộng đồng công nghệ bởi khả năng phát triển ứng dụng vượt trội.

## Mục lục

[1. Node.js hoạt động thế nào?](#1-nodejs-hoạt-động-thế-nào)

[2. Quản lý package trong ứng dụng Node.js?](#2-quản-lý-package-trong-ứng-dụng-nodejs)

[3. Node.js có tốt hơn các framework khác?](#3-nodejs-có-tốt-hơn-các-framework-khác)


[5. Các tính năng thời gian của Node.js?](#5-các-tính-năng-thời-gian-của-nodejs)

### 1. Node.js hoạt động thế nào?

Node hoàn toàn theo cơ chế event-driven. Về cơ bản server bao gồm một luồng duy nhất xử lý từ sự kiện này đến sự kiện khác.

Một yêu cầu mới đến là một loại sự kiện. Server bắt đầu xử lý nó và khi có hoạt động blocking IO, nó sẽ không đợi cho đến khi hoàn thành mà thay vào đó sẽ đăng ký một hàm callback. Sau đó, server ngay lập tức bắt đầu xử lý một sự kiện khác (có thể là một yêu cầu khác). Khi hoạt động IO kết thúc, đó là một loại sự kiện khác và server sẽ xử lý nó (tức là tiếp tục làm việc theo yêu cầu) bằng cách thực hiện lệnh callback ngay khi có thời gian.

Vì vậy, server không bao giờ cần tạo thêm các luồng hoặc chuyển đổi giữa các luồng, có nghĩa là nó có rất ít chi phí. Nếu bạn muốn sử dụng đầy đủ nhiều lõi phần cứng, bạn chỉ cần bắt đầu nhiều đối tượng node.js

Nền tảng Node.js không tuân theo mô hình đa luồng. Mà nó theo mô hình đơn luồng với Event Loop. Mô hình xử lý trong Node.js chủ yếu dựa trên mô hình JavaScript Event và cơ chế callback.

Các bước trong mô hình xử lý đơn luồng với Event Loop:
- Client gửi yêu cầu đến web server.
- Web server Node.js duy trì một Thread pool để cung cấp dịch vụ cho các yêu cầu từ client.
- Node.js nhận các yêu cầu này và đặt nó vào một hàng đợi. Gọi là Event Queue.
- Trong Nodejs có một thành phần là Event Loop. Nó sử dụng một vòng lặp để nhận yêu cầu và xử lý chúng.
- Event Loop sử dụng một luồng duy nhất. Nó được gọi là trái tim của Node.js
- Event Loop kiểm tra yêu cầu có ở trong Event Queue. Nếu khong nó sẽ đợi cho đến khi yêu cầu đến.
- Nếu có, nó lấy yêu cầu từ Event Queue:
    - Nó bắt đầu xử lý yêu cầu đó.
    - Nếu yêu cầu đó không phải là blocking IO, thì nó xử lý và chuẩn bị phản hồi để gửi về client.
    - Nếu nó cần vài thao tác blocking IO như tương tác với cơ sở dữ liệu, hệ thống file, mạng thì nó sẽ có cách tiếp cận khác:
        + Kiểm tra luồng khả dụng từ Thread Pool
        + Chọn luồng và gán nó cho yêu cầu client.
        + Các luồng này nhận yêu cầu và xử lý chúng thực hiện hành động blocking IO, chuẩn bị phản hồi và gửi nó về Event Loop.
        + Event Loop lấy nó và gửi phản hồi đó về lại client.
    
### 2. Quản lý package trong ứng dụng Node.js?

Khi thảo luận về Node js thì một điều chắc chắn không nên bỏ qua là xây dựng package quản lý sử dụng các công cụ NPM mà mặc định với mọi cài đặt Node js. Ý tưởng của mô-đun NPM là khá tương tự như Ruby-Gems: một tập hợp các hàm có sẵn có thể sử dụng được, thành phần tái sử dụng, tập hợp các cài đặt dễ dàng thông qua kho lưu trữ trực tuyến với các phiên bản quản lý khác nhau. Bên cạnh npm cũng có thể sử dụng yarn với bộ chức năng tương tự.

### 3. Node.js có tốt hơn các framework khác?

- **Bất đồng bộ**: Đặc điểm đầu tiên của Nodejs là tính bất đồng bộ. Node.js không cần đợi API trả dữ liệu về, vậy nên mọi APIs nằm trong thư viện Node.js đều không được đồng bộ, hiểu đơn giản là chúng không hề blocking (khóa). Server có cơ chế riêng để gửi thông báo và nhận phản hồi về các hoạt động của Node.js và API đã gọi.
- **Tốc độ nhanh**: Với phần core phía dưới lập trình gần như toàn bộ bằng ngôn ngữ C++, kết hợp với V8 Javascript Engine mà Google Chrome cung cấp, tốc độ vận hành, thực hiện code của thư viện Node.js diễn ra rất nhanh.
- **Đơn giản/Hiệu quả**: Tiến trình vận hành của Node.js đơn giản song lại mang đến hiệu năng cao nhờ ứng dụng mô hình single thread và các sự kiện lặp. Một loạt cơ chế sự kiện cho phép server trả về phản hồi bằng cách không block, đồng thời tăng hiệu quả sử dụng. Các luồng đơn cung cấp dịch vụ cho nhiều request hơn hẳn Server truyền thống.
- **Không đệm**: Nền tảng Node.js không có vùng đệm, tức không cung cấp khả năng lưu trữ dữ liệu buffer.
- **Có giấy phép**: Đây là nền tảng đã được cấp giấy phép, phát hành dựa trên MIT License.


### 5. Các tính năng thời gian của Node.js?

Các hàm Set Timer: