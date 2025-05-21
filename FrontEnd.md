transpile
First things first

Bài viết tổng quan về front end ( web ).

1: Thành phần 
Front-End bao gồm ba công nghệ chính hợp tác tạo nên giao diện web: 
- HTML xây dựng cấu trúc và nội dung. HTML là “bộ khung” của trang, xác định các phần tử (đoạn văn, tiêu đề, hình ảnh, v.v.)
- CSS điều khiển trình bày và bố cục. CSS định kiểu (màu sắc, font, khoảng cách, bố cục)	
- JavaScript bổ sung tính năng tương tác và động. JavaScript xử lý logic, phản hồi sự kiện người dùng (click, nhập liệu) và cập nhật DOM. 
        +  TypeScript: là ngôn ngữ mở rộng của JavaScript thêm tính năng kiểu tĩnh (static typing). TypeScript kiểm tra kiểu tại thời gian biên dịch và cung cấp công cụ IDE mạnh hơn (gợi ý mã, bắt lỗi sớm). Mã TS được biên dịch về JS để chạy ở trình duyệt.
Ví dụ:
JavaScript:
let age = 30;
age = "thirty"; // Không báo lỗi, nhưng dễ gây bug
TypeScript:
let age: number = 30;
age = "thirty"; // ❌ Lỗi: Type 'string' is not assignable to type 'number'
Tóm tắt sự khác biệt chính:
Tính năng	JavaScript	TypeScript
Kiểu dữ liệu tĩnh	❌ Không	                        ✅ Có
Phát hiện lỗi sớm	❌ Không	                ✅ Có lúc biên dịch
Khai báo biến có kiểu rõ ràng	❌ Không	                        ✅ Có
Hàm có kiểm tra tham số và kiểu trả về	❌ Không	                        ✅ Có
Hướng dẫn lập trình viên tốt hơn	❌ Không	✅ Có (nhờ IntelliSense, type checking)

+ DOM (Document Object Model): là mô hình đối tượng tài liệu, đại diện cây cấu trúc HTML của trang, cho phép JavaScript truy xuất và thay đổi nội dung, cấu trúc hoặc style động trên trang. Ví dụ, DOM cho phép tìm phần tử theo ID và cập nhật giá trị hoặc sự kiện cho chúng.
+ BOM (Browser Object Model): bao gồm các đối tượng mà JavaScript dùng để tương tác với trình duyệt, như window, history, location… BOM không chuẩn hóa nhưng cho phép kiểm soát cửa sổ trình duyệt, URL, lịch sử… mà DOM không có.
Vậy chúng ta đã có 3 thành phần chính để cấu tạo nên các website cơ bản nhưng các phần phần trên chạy ở đâu ? Hãy đến phần 2 để rõ hơn .





2: Môi trường 
Tại sao HTML/CSS/JS cần môi trường để chạy?
Vì bản thân chúng không phải là chương trình độc lập hoặc ứng dụng máy tính, mà chỉ là:
•	HTML: Cấu trúc nội dung.
•	CSS: Trình bày giao diện.
•	JavaScript: Tạo động, tương tác.
Chúng cần môi trường hiểu và xử lý chúng – cụ thể là:
•	HTML/CSS cần một engine render (trình duyệt) để vẽ UI.
•	JS cần một engine như V8 (Chrome) hay SpiderMonkey (Firefox) để chạy lệnh.
Trình duyệt Web (Browser) – Môi trường gốc để chạy

Thành phần	Vai trò của trình duyệt
HTML	Trình duyệt phân tích cú pháp (parse), xây DOM
CSS	Trình duyệt xây dựng CSSOM và render giao diện
JavaScript	Trình duyệt thực thi mã JS bằng engine (VD: V8 của Chrome)
Ví dụ: Khi bạn mở file index.html bằng Chrome, trình duyệt sẽ:
•	Đọc HTML ➝ xây DOM Tree.
•	Đọc CSS ➝ xây CSSOM Tree.
•	Kết hợp DOM + CSSOM ➝ tạo Render Tree để vẽ UI.
•	Đọc và thực thi JavaScript để tạo tương tác, xử lý logic động.
TypeScript không chạy trực tiếp trong trình duyệt, mà cần biên dịch sang JavaScript (transpile) trước đó (thường bằng tsc, Webpack, hoặc Vite).
Như vậy từ các file html, css, js tĩnh và môi trường thì ta đã có thể xây dựng được các trang web cực kỳ sống động rồi. Nhưng những trang web hiện tại có nhiều trang, rất nhiều thành phần, hoạt ảnh vì vậy để có thể dễ dàng hơn trong việc lập trình ta tới phần tiếp theo là Frameword và Lib FrontEnd.

3.Frameword và Lib	
Đầu tiên ta cần phân biệt giữa Library và Framework :

Library = Bộ công cụ (Toolbox)
Ví dụ: Bạn có một bộ dụng cụ sửa xe: có tua vít, cờ lê, búa, kìm…
Bạn chọn ra từng món phù hợp để sửa cái bạn muốn: bánh xe, dây điện, đèn pha...
Bạn điều khiển quá trình sửa – bạn là người ra quyết định.
Framework = Bộ máy/Khung sườn hoàn chỉnh
Ví dụ: Một dây chuyền lắp ráp ô tô có sẵn:
Băng chuyền chạy sẵn.
Có các module đã định nghĩa (gắn động cơ, lắp bánh, kiểm tra an toàn…)
Việc của bạn: “gắn” các bộ phận riêng (code, logic, UI...) vào đúng chỗ.
Đặc điểm	Library (Bộ công cụ)	Framework (Bộ máy hoàn chỉnh)
Ai kiểm soát luồng	Bạn (code của bạn gọi thư viện)	Framework (framework gọi code của bạn)
Mức độ linh hoạt	Cao, tuỳ chỉnh tuỳ ý	Thấp hơn, phải theo quy tắc có sẵn
Mục tiêu	Giải quyết bài toán nhỏ/lẻ	Xây dựng cả hệ thống front-end
Ví dụ	React, Lodash, Axios	Angular, Vue, Svelte, Next.js, Nuxt.js
Ẩn dụ	Bộ đồ nghề – bạn chủ động sửa/lắp	Dây chuyền – bạn chỉ gắn phần mình vào thôi

Các Library:
+ Các Library UI giúp thiết kế UI nhanh, chuẩn hóa : Một số Lib có sẵn giúp việc tùy chỉnh giao diện được dễ dàng, responsive hơn như: Tailwind, Ant Design, Bootstrap, …
+ Các thư viện tiện ích như 
	Axios: Gọi API
	Three.js: Đồ họa 3D WebGL
Chart.js, Recharts: Vẽ biểu đồ, data visualization
+ … và còn rất nhiều lib khác nhau tùy theo nhu cầu sử dụng
Các Framework:
React.js (Library – Facebook)
Kiểu: Component-based
Ưu điểm: Dễ tùy biến, cộng đồng cực lớn, tương thích với TypeScript, React Native
Nhược: Không built-in router/store – phải dùng thêm Redux, React Router,...
Dùng với: Next.js, Redux, Zustand, TailwindCSS...
Vue.js (Progressive Framework – Evan You)
Kiểu: Kết hợp giữa Angular và React, dễ học hơn
Ưu điểm: Template HTML, reactive data, CLI mạnh, cộng đồng Đông Á
Dùng với: Pinia, Vue Router, Vite, Quasar, Nuxt.js
Angular (Framework – Google)
Kiểu: Full-fledged (Router, HTTP, Form, DI có sẵn)
Viết bằng: TypeScript hoàn toàn
Dùng tốt trong: Enterprise, ứng dụng lớn, ngân hàng
Nặng hơn, steeper learning curve
Svelte (Compiler-based Framework)
Đặc điểm: Không dùng Virtual DOM – compile trực tiếp thành JS native
Siêu nhẹ, syntax gần giống HTML/JS
Dự án lớn: SvelteKit
SolidJS
Giống Svelte về hiệu năng nhưng vẫn giữ JSX như React
Reactive bằng signal, không dùng Virtual DOM

Để có thể đi chi tiết hơn về framework thì mình sẽ đề cập tới ở 1 bài khác 
