###### **Bài 4 : Sự cố xung đột phiên đăng nhập trên trình duyệt**

**Phần 1**

|tiêu chí so sánh |Incognito Mode (Ẩn danh)|Dedicated Profile (Cấu hình độc lập)|
|-|-|-|
|cơ chế lưu trữ trạng thái|Tạm thời trên RAM / Bộ nhớ biến động: Toàn bộ cookie, session token, lịch sử và cache chỉ tồn tại trong vòng đời của cửa sổ ẩn danh đó. Khi đóng cửa sổ cuối cùng, dữ liệu tự động bị xóa.|Lâu dài trên ổ cứng (Disk-based): Lưu trữ toàn bộ dữ liệu (cookies, lịch sử, extension, mật khẩu đã lưu, bookmark riêng) vào một thư mục profile độc lập chuyên biệt trên ổ cứng.|
|độ bền phiên đăng nhập |Kém: Không thể duy trì phiên đăng nhập lâu dài giữa các lần mở trình duyệt. Mỗi lần mở mới yêu cầu đăng nhập lại từ đầu (MFA/OTP), dễ gây xung đột khi mở song song nhiều tab với các tài khoản khác nhau.|Cao \& Ổn định: Duy trì phiên đăng nhập vĩnh viễn (hoặc đến khi token hết hạn) cho từng môi trường riêng biệt, hoàn toàn cô lập cookie giữa các tài khoản.|
|khả năng tùy biến và tiện ích|Bị hạn chế (mặc định vô hiệu hóa phần lớn extension trừ khi được cấp phép thủ công từng extension trong cài đặt ẩn danh).|Đầy đủ: Mỗi profile có thể cài đặt riêng các extension, theme, cấu hình proxy và công cụ nhà phát triển (DevTools) độc lập.|
|rủi ro xung đột và rog rỉ|Dễ bị ghi đè phiên nếu các tab cùng domain truy cập chia sẻ chung bộ nhớ cookie tạm thời của phiên ẩn danh.|Triệt tiêu hoàn toàn rủi ro: Các profile hoạt động trong các không gian tiến trình và thư mục dữ liệu riêng biệt.|

**Phần 2**

Bước 1. Thiết lập 3 Profile độc lập trên Google ChromeMở Google Chrome, nhấp vào Ảnh đại diện (hoặc biểu tượng Profile) ở góc trên bên phải cửa sổ trình duyệt.Chọn Thêm (Add) ở dưới cùng danh sách profile.Chọn Đăng nhập (Sign in) nếu muốn liên kết tài khoản Google riêng biệt để đồng bộ Cloud Sync, hoặc chọn Tiếp tục mà không cần tài khoản (đối với profile kiểm thử Sandbox).Lần lượt tạo 3 cấu hình với tên định danh:Profile 1: Personal (Dành cho tài khoản cá nhân, giải trí).Profile 2: Work (Dành cho công việc quản trị hệ thống GreenLog, kết nối tài khoản công ty).Profile 3: Sandbox (Dành cho môi trường kiểm thử, không đồng bộ tài khoản cá nhân để đảm bảo an toàn tuyệt đối).Bước 2. Cấu hình bảo mật chống rò rỉ dữ liệu (Ràng buộc trình chiếu)Tách biệt tài khoản Google: Profile Work phải được đăng nhập bằng email công ty/quản trị. Profile Personal dùng email cá nhân. Điều này đảm bảo lịch sử web, mật khẩu lưu trữ trong trình quản lý mật khẩu (Password Manager) của Profile cá nhân không bao giờ bị đồng bộ hay xuất hiện khi đang trình chiếu hoặc thao tác trên Profile công việc.Kiểm tra nhanh qua DevTools (F12):Nhấn F12 hoặc Ctrl + Shift + I trên mỗi profile $\\rightarrow$ chuyển sang tab Application $\\rightarrow$ mục Storage (Cookies, Local Storage, Session Storage). Bạn sẽ thấy các phiên cookie hoàn toàn khác nhau và không bị ghi đè chéo giữa các cửa sổ profile.Bước 3. Tổ chức cấu trúc Bookmark khoa họcĐể truy cập nhanh các công cụ cốt lõi (GitHub, Canvas LMS, Portal công ty) mà không tốn diện tích hiển thị, hãy xây dựng thanh Bookmark theo cấu trúc cây thư mục trực quan, tối ưu hóa theo phong cách favicon-only (chỉ hiển thị icon thu gọn) trên thanh công cụ:Plaintext📁 Thanh dấu trang (Bookmarks Bar)

├── 📁 01\_Work\_Core/

│   ├── 🔗 GreenLog Portal (favicon)

│   ├── 🔗 Quản trị hệ thống (favicon)

│   └── 🔗 Jira / Task Management (favicon)

├── 📁 02\_Academic/

│   ├── 🔗 Canvas LMS (favicon)

│   ├── 🔗 GitHub Repository (favicon)

│   └── 🔗 Cổng thông tin sinh viên (favicon)

└── 📁 03\_Dev\_Tools/

&#x20;   ├── 🔗 LeetCode (favicon)

&#x20;   └── 🔗 StackEdit / Markdown (favicon)

Bước 4. Quy trình đồng bộ hóa Cloud Sync bảo vệ Bookmark khi đổi máyTrong mỗi Profile (Personal và Work), nhấn vào biểu tượng 3 chấm $\\rightarrow$ Cài đặt (Settings) $\\rightarrow$ Bạn và Google (You and Google) $\\rightarrow$ Dịch vụ của Google và Chrome (Google services).Bật tính năng Đồng bộ hóa (Sync) và chọn Đồng bộ hóa mọi thứ (hoặc tùy chỉnh chọn phần Dấu trang - Bookmarks).Cơ chế hoạt động: Khi bạn đổi sang máy tính mới, chỉ cần đăng nhập lại tài khoản Google tương ứng vào đúng profile đó, toàn bộ cấu trúc cây thư mục bookmark, tiện ích mở rộng và lịch sử cấu hình sẽ tự động đồng bộ xuống thiết bị mới qua đám mây mà không sợ thất lạc.

**Phần 3**

Luôn mở cửa sổ Khách thay vì đăng nhập tài khoản cá nhân:



Trên màn hình chọn profile của Chrome, nhấp vào Duyệt với tư cách Khách (Guest).



Lý do: Chế độ khách tạo ra một không gian duyệt web hoàn toàn tạm thời, cách ly hoàn toàn với các profile có sẵn trên máy và tự động xóa sạch toàn bộ lịch sử, cookie, file cache ngay khi cửa sổ Guest được đóng lại.



Tuyệt đối không lưu mật khẩu (Save Passwords):



Khi trình duyệt hỏi "Bạn có muốn lưu mật khẩu này không?", luôn chọn Không bao giờ (Never) hoặc tắt hẳn trình quản lý mật khẩu trên phiên làm việc của khách.



Xác thực đa yếu tố (MFA / 2FA) và Đăng xuất thủ công:



Nếu bắt buộc phải đăng nhập tài khoản quản trị GreenLog trên máy công cộng, hãy sử dụng mã OTP sinh ra từ ứng dụng di động cá nhân.



Quy tắc vàng: Ngay sau khi hoàn thành phiên làm việc, tiến hành Đăng xuất (Log out) thủ công khỏi hệ thống trước, sau đó mới tắt cửa sổ trình duyệt Guest để vô hiệu hóa hoàn toàn token phiên làm việc trên thiết bị lạ.

