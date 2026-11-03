###### **Bài 3 : Xử lý lỗi bố cục giao diện hệ thống theo dõi xe**

**Phần 1**

Content: Kích thước nội dung bên trong (Ví dụ: width: 320px, height: 180px).



Padding: Khoảng cách đệm bên trong (Ví dụ: top/bottom: 16px, left/right: 16px).



Border: Đường viền bao quanh (Ví dụ: 1px solid #e0e0e0).



Margin: Khoảng cách bên ngoài (Ví dụ: margin-bottom: -50px hoặc margin-top: 0px gây hiện tượng âm, kéo phần tử dịch chuyển đè lên vùng khác).

Nguyên nhân: Khung dùng position: absolute/fixed hoặc thiếu display: flex/grid ở thẻ cha, khiến phần tử thoát khỏi dòng chảy tự nhiên và bị chồng lấn.

**Phần 2**

\### Phương án: Sử dụng Flexbox trên phần tử cha (Container)



```css

/\* Chọn phần tử cha chứa cả 2 khung \*/

.route-container {

&#x20;   display: flex;

&#x20;   flex-direction: column; /\* Hoặc row tùy theo thiết kế \*/

&#x20;   gap: 16px;             /\* Tạo khoảng cách chuẩn giữa 2 khung \*/

&#x20;   position: relative;

}



/\* Đảm bảo các khung nằm đúng dòng chảy \*/

.driver-info {

&#x20;   position: static;      /\* Gỡ bỏ absolute/fixed nếu không cần thiết \*/

&#x20;   width: 100%;

&#x20;   box-sizing: border-box;

}



.station-list {

&#x20;   position: static;

&#x20;   width: 100%;

&#x20;   box-sizing: border-box;

}

**Phần 3**



Giả định câu hỏi: Thay đổi margin nhưng phần tử vẫn không di chuyển, hãy kiểm tra xem có thuộc tính nào đang ghi đè ở tầng Browser Engine không?



Khi thay đổi margin mà phần tử hoàn toàn không dịch chuyển, nguyên nhân và cách kiểm tra tại tab Styles / Computed trong DevTools bao gồm:



Phần tử đang bị định vị độc lập (position: absolute hoặc position: fixed):



Giải thích: Các phần tử mang thuộc tính position: absolute hoặc fixed bị đưa ra khỏi dòng chảy tài liệu (normal flow). Do đó, các thuộc tính margin theo chiều dọc (đặc biệt là margin-top / margin-bottom) thường không làm xê dịch vị trí của nó so với các phần tử xung quanh theo ý muốn thông thường.



Cách kiểm tra: Nhìn vào tab Computed, kiểm tra thuộc tính position. Nếu khác static hoặc relative, margin sẽ không hoạt động theo cách đẩy phần tử bình thường.



Thuộc tính hiển thị là display: inline:



Giải thích: Các thẻ HTML mặc định là inline (như <span>, <a>) sẽ không nhận giá trị margin-top và margin-bottom.



Cách kiểm tra: Kiểm tra display trong tab Computed. Nếu là inline, cần đổi thành display: inline-block hoặc display: block.



Bị ghi đè bởi độ ưu tiên (Specificity) hoặc cờ !important:



Giải thích: Có một đoạn CSS khác trong tệp CSS hệ thống hoặc framework (như Bootstrap, Tailwind,...) đang định nghĩa margin với độ ưu tiên cao hơn hoặc có gắn !important.



Cách kiểm tra: Trong tab Styles, quan sát xem quy tắc margin bạn vừa nhập có bị gạch ngang (strikethrough) hay không. Nếu bị gạch ngang, nghĩa là có đoạn code khác đang ghi đè. Cần kiểm tra xem đoạn code ghi đè nằm ở dòng mấy, file nào được hiển thị ngay bên cạnh.

