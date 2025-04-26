Bean lifecycle:
"Bean là một đối tượng Java được Spring IoC container khởi tạo, cấu hình và quản lý vòng đời.
Bean có thể được tạo ngay khi ứng dụng khởi động (eager) hoặc khi được yêu cầu (lazy/prototype), và có thể đại diện cho bất kỳ thành phần nào trong ứng dụng."

Meaning: Bean đóng vai trò "đơn vị cơ bản" trong Spring. Nó không chỉ đơn thuần là một instance mà còn được tích hợp sẵn các tính năng mạnh mẽ nhờ Spring IoC container:
- Đóng gói chức năng (Service, DAO, Controller)
- Chứa cấu hình (DataSource, TransactionManager)
- Cung cấp dữ liệu (DTO, Entity dạng @Bean)
  
Spring Bean ra đời để giải quyết 4 vấn đề lớn:

1. Thoát Khỏi Sự Phụ Thuộc Cứng (Tight Coupling)
     
