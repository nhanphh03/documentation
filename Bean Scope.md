*Bean scope mô tả  **cách Spring container quản lý các instance của bean** . Khi một bean được định nghĩa trong Spring container, tùy thuộc vào scope mà Spring sẽ tạo một hoặc nhiều instance của bean đó.*

Các loại bean scope phổ biến:

**Singleton (Mặc định) - Một service xử lý nghiệp vụ chung, không giữ dữ liệu riêng của từng user.**

* **Ý nghĩa:** Chỉ tạo **duy nhất một instance** cho mỗi bean trong toàn bộ Spring container.
* **Dùng khi:** Bean không lưu trạng thái riêng biệt giữa các user/reques

**Prototype - Một bean lưu trạng thái tạm thời và không chia sẻ được. VD: thông tin của một người dùng đang thao tác.**

* **Ý nghĩa:** Mỗi lần bean được yêu cầu thì Spring sẽ tạo  **một instance mới** .
* **Dùng khi:** Cần nhiều instance độc lập (ví dụ: mỗi người dùng cần một instance khác nhau).

**Request (chỉ dùng trong ứng dụng web) - Một bean dùng để lưu thông tin người dùng gửi trong form, chỉ tồn tại trong một request HTTP.**

* **Ý nghĩa:** Tạo một bean mới cho mỗi HTTP request.
* **Dùng khi:** Bean lưu thông tin liên quan đến một request cụ thể (ví dụ: thông tin người dùng gửi form).

**Session (chỉ dùng trong ứng dụng web) - Lưu trạng thái đăng nhập người dùng (user login info), tồn tại suốt phiên làm việc của user.**

* **Ý nghĩa:** Mỗi session của người dùng sẽ có một instance riêng biệt.
* **Dùng khi:** Bean cần giữ trạng thái xuyên suốt phiên làm việc của user.

**Application (web) - Lưu thông tin cấu hình toàn hệ thống mà các request đều cần dùng, ví dụ cấu hình hiển thị.**

* **Ý nghĩa:** Một bean được chia sẻ trong toàn bộ lifecycle của một web application (trong servlet context).

| Scope           | Instance  | Ngữ cảnh              | Ví dụ thực tế                          |
| --------------- | --------- | ----------------------- | ------------------------------------------ |
| `singleton`   | 1         | Toàn container         | Service xử lý logic chung                |
| `prototype`   | N         | Mỗi lần gọi          | Form tạm lưu thông tin người dùng    |
| `request`     | 1/request | Web - mỗi HTTP request | Dữ liệu request, token,...               |
| `session`     | 1/session | Web - mỗi session user | Dữ liệu đăng nhập user                |
| `application` | 1/app     | Web - toàn app         | Cấu hình dùng chung toàn hệ thống    |
| `websocket`   | 1/ws      | WebSocket session       | Dữ liệu người dùng trong kênh socket |


Spring không thể inject trực tiếp `@RequestScoped` hoặc `@SessionScoped` bean vào **singleton bean** (ở đây là `ScopeController`), vì chúng  **chỉ tồn tại trong ngữ cảnh HTTP request/session** , trong khi `ScopeController` được tạo lúc ứng dụng start.

V1

```java
@RestController
@RequestMapping("/scope")
public class ScopeController {

    private final SingletonBean singletonBean;

    private final PrototypeBean prototypeBean;

    private final RequestBean requestBean;

    private final SessionBean sessionBean;

    public ScopeController(SingletonBean singletonBean, PrototypeBean prototypeBean, RequestBean requestBean, SessionBean sessionBean) {
        this.singletonBean = singletonBean;
        this.prototypeBean = prototypeBean;
        this.requestBean = requestBean;
        this.sessionBean = sessionBean;
    }

    @GetMapping
    public String getBeanIds() {
        return sout(singletonBean.getId(), prototypeBean.getId(), requestBean.getId(), sessionBean.getId());
    }
    @GetMapping("/v2")
    public String getBeanIdsV2() {
        return sout(singletonBean.getId(), prototypeBean.getId(), requestBean.getId(), sessionBean.getId());
    }
    private String sout(String singletonBean, String prototypeBean, String requestBean, String sessionBean) {
        return String.format(
                "Singleton: %s\nPrototype: %s\nRequest: %s\nSession: %s",
                singletonBean,
                prototypeBean,
                requestBean,
                sessionBean
        );
    }
```

```
//  getBeanIds :  Singleton: 0d22c69a-eef0-43ca-af79-491c4dfac9b0 Prototype: 352bd6b7-f2be-4d3d-9fa3-1ceef51dff4d Request: 4b21f18d-02a6-4d1d-a4b9-d90337434d04 Session: 5b39f73b-71fa-4bb4-9c1b-a97268938423
//  getBeanIdsV2: Singleton: 0d22c69a-eef0-43ca-af79-491c4dfac9b0 Prototype: 352bd6b7-f2be-4d3d-9fa3-1ceef51dff4d Request: 8382f7c0-3b29-4f8c-a47c-6d307944dd97 Session: 5b39f73b-71fa-4bb4-9c1b-a97268938423
```

V2: 

```java
import com.example.demo.bean.PrototypeBean;
import com.example.demo.bean.RequestBean;
import com.example.demo.bean.SessionBean;
import com.example.demo.bean.SingletonBean;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/scope/v2")
public class ScopeControllerV2 {

    private final SingletonBean singletonBean;

    private final PrototypeBean prototypeBean;

    private final RequestBean requestBean;

    private final SessionBean sessionBean;

    public ScopeControllerV2(SingletonBean singletonBean, PrototypeBean prototypeBean, RequestBean requestBean, SessionBean sessionBean) {
        this.singletonBean = singletonBean;
        this.prototypeBean = prototypeBean;
        this.requestBean = requestBean;
        this.sessionBean = sessionBean;
    }

    @GetMapping
    public String getBeanIds() {
        return sout(singletonBean.getId(), prototypeBean.getId(), requestBean.getId(), sessionBean.getId());
    }
    private String sout(String singletonBean, String prototypeBean, String requestBean, String sessionBean) {
        return String.format(
                "Singleton: %s\nPrototype: %s\nRequest: %s\nSession: %s",
                singletonBean,
                prototypeBean,
                requestBean,
                sessionBean
        );
    }
}
```


Result:

`http://localhost:8080/scope/v2 //Singleton: c1edcd72-0981-4899-a589-3b8271eb34df Prototype: 71c6cb27-28fe-4b80-a770-a1eebcb94d39 Request: 26764f46-0195-4528-87c6-87fb46fd9152 Session: bd4274a0-3c17-437b-aa04-419ff4f9bc33`

`http://localhost:8080/scope //Singleton: c1edcd72-0981-4899-a589-3b8271eb34df Prototype: 792b9e08-8311-4d9b-a834-e132f67ff49d Request: cf35844b-a636-4161-9ddd-fc4f40263854 Session: bd4274a0-3c17-437b-aa04-419ff4f9bc33 `

`Incognito Tab //Singleton: c1edcd72-0981-4899-a589-3b8271eb34df Prototype: 792b9e08-8311-4d9b-a834-e132f67ff49d Request: d2fe48e1-b622-4c68-9d8f-0cff4efbe387 Session: c1cb449e-ce0f-49d2-aa2f-3ce059a1e80c`


Vì controller là một **singleton,** và bạn inject prototype bean **một lần duy nhất khi controller được tạo** => nên `prototypeBean` chỉ được tạo đúng 1 lần duy nhất khi controller khởi tạo → các request sau vẫn dùng  **cùng instance** .

Thay thế để có thể sử dụng được prototype bean: 

- Dùng `ObjectProvider` : là cách  **lười khởi tạo (lazy)**, chỉ tạo bean khi  `.getObject()` → mỗi request tạo `PrototypeBean` mới.

  ```java
    private final ObjectProvider<PrototypeBean> prototypeBeanProvider;

    PrototypeBean prototypeBean = prototypeBeanProvider.getObject(); 
  ```
- Dùng `ApplicationContext` : Hiệu quả tương đương nhưng dùng `ApplicationContext` sẽ phụ thuộc mạnh hơn vào Spring container (ít clean hơn `ObjectProvider`).
