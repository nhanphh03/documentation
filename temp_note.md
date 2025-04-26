Bean lifecycle:
"Bean là một đối tượng Java được Spring IoC container khởi tạo, cấu hình và quản lý vòng đời.
Bean có thể được tạo ngay khi ứng dụng khởi động (eager) hoặc khi được yêu cầu (lazy/prototype), và có thể đại diện cho bất kỳ thành phần nào trong ứng dụng."

Meaning: Bean đóng vai trò "đơn vị cơ bản" trong Spring. Nó không chỉ đơn thuần là một instance mà còn được tích hợp sẵn các tính năng mạnh mẽ nhờ Spring IoC container:
- Đóng gói chức năng (Service, DAO, Controller)
- Chứa cấu hình (DataSource, TransactionManager)
- Cung cấp dữ liệu (DTO, Entity dạng @Bean)
  
Spring Bean ra đời để giải quyết 4 vấn đề lớn:

1. Thoát Khỏi Sự Phụ Thuộc Cứng (Tight Coupling)

-- Code cũ

    class UserService {
        private UserRepository repo = new UserRepositoryImpl(); // Khớp nối cứng
    }

-- Spring Bean 

    @Bean
    public UserService userService(UserRepository repo) { // DI tự động
        return new UserServiceImpl(repo); // Khớp nối lỏng
    }

→ Thay đổi implementation mà không cần sửa code (VD: đổi từ MySQL sang MongoDB chỉ bằng config).

![img.png](img.png)
Bean sinh ra để biến "ứng dụng phức tạp" thành "tập hợp các component lỏng lẻo", giúp:

Dễ bảo trì: Thay đổi component mà không ảnh hưởng phần khác
Dễ test: Có thể mock dependency dễ dàng
Giảm boilerplate code: Tự động hóa DI, lifecycle management

Triết lý: Spring Bean là hiện thực của nguyên lý "Don't call us, we'll call you" (Hollywood Principle) - Bạn khai báo "cần gì", Spring sẽ tự động cung cấp.


----------------------------


Nếu có tài nguyên cần giải phóng (DB connection, file handle...), nên implement DisposableBean hoặc dùng @PreDestroy

Với Prototype Bean cần quản lý tài nguyên, có thể dùng Object Pool pattern hoặc tự theo dõi để hủy

Đừng trông chờ Spring hủy Bean tự động như GC, vì Spring được thiết kế để quản lý chủ động vòng đời Bean

