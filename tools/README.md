## Table of content

[1. RabbitMQ](#1-rabbitmq)   
[2. Kafka](#2-kafka)  
[3. Elasticsearch](#3-elasticsearch)  
[4. Logstash](#3-logstash)   
[5. Kibana](#3-kibana)    
[6. Prometheus](#3-prometheus)      
[7. Grafana](#3-grafana)         


### 1. RabbitMQ
### 2. Kafka
### 3. Elasticsearch

ElasticSearch là công cụ để lưu trữ, xử lý và tìm kiếm dữ liệu nhanh chóng, hỗ trợ full-text search,v v…

**Quy trình hoạt động:**

- Ghi dữ liệu vào ElasticSearch:
    1. ElasticSearch sẽ tạo 1 chỉ mục (Index). Chỉ mục giống như 1 table trong RDBMS 
    2. Đưa các document vào chỉ mục. 1 document trong ElasticSearch thường là 1 đơn vị dữ liệu có dạng JSON với các field chứ thông tin cụ thể (mỗi document tương tự như 1 row trong RDBMS)
    3. ElasticSearch chia các Index thành các Shard (mảnh dữ liệu) để phân tán dữ liệu giúp tối ưu hoá tìm kiếm. Ngoài ra ElasticSearch còn tạo ra các bản sao của các shard để giúp cho dữ liệu luôn available khi có bất kỳ node nào gặp sự cố, giúp hệ thống đáng tin cậy, có tính sẵn sàng cao, không bị gián đoạn
    4. Sau đó, ElasticSearch tiến hành tạo các Inverted Index (chỉ mục đảo ngược) từ các từ trong các document của Index. Inverted Index đóng vai trò như 1 bảng tra cứu, giúp ElasticSearch nhanh chóng tìm ra các kết quả chứa các từ khoá mà người dùng yêu cầu. 
        1. ElasticSearch sử dụng các analyzers để loại bỏ các từ không quan trọng (vd như các từ nối, giới từ, v.v..) trong các field của document, chuẩn hoá từ ngữ, gộp các từ đồng nghĩa
        2. Sau đó, tokenizer tiếp nhận và chi nhỏ các từ còn lại thành các đơn vị từ khoá (token). Ví dụ: nếu giá trị field `name` chứa từ khoá “giày thế thao” thì nó sẽ được phân tách thành 2 từ là “giày” và “thể thao”. Những từ khoá này sẽ được lưu vào Inverted Index. Mỗi từ khoá sẽ được ánh xạ đến các document chứa từ khoá đó
        3. Sau khi Inverted Index được tạo, nếu người dùng truy vấn, ví dụ như “giày thế thao”, ElasticSearch sẽ không phải quét qua toàn bộ các document, thay vào đó nó sẽ tra cứu Inverted Index để tìm ra các document chứa từ khoá “giày” và “thể thao” rồi trả về kết quả phù hợp. Trong quá trình này, ElasticSearch sử dụng các thuật toán như TF-ID hoặc BM25 để tính toán độ liên quan của từng document đối với truy vấn và sắp xếp kết quả theo độ chính xác. Cuối cùng, ElasticSearch trả về danh sách kết quả có độ chính xác cao nhất với truy vấn của người dùng

### 4. Logstash
### 5. Kibana
### 6. Prometheus
### 7. Grafana