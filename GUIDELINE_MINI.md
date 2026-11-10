# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | xe con (sedan, SUV, taxi, xe bán tải/pickup); phân loại theo loại xe, không dựa duy nhất vào số chỗ | xe con, sedan, SUV, taxi, xe bán tải |
| `truck` | xe tải chuyên chở hàng (thùng hở, thùng kín, đầu kéo/container); không bao gồm pickup được xếp vào `car` | xe tải chở hàng, xe đầu kéo, xe container |
| `bus` | xe buýt và xe khách dạng bus (bao gồm minibus chở khách), không chỉ xe cỡ lớn | xe bus công cộng, xe khách du lịch, xe bus 2 tầng |
| `van` | xe van/minivan chở người hoặc hàng, thường có thân hộp; phải phân biệt với xe buýt/minibus và xe con | mini van, xe van chở hàng |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: box ôm sát phần vật thể thực tế nhìn thấy trong ảnh, không ước lượng thêm phần đã bị che khuất hay nằm ngoài ảnh
- Vật thể bị che khuất: "occluded" vật thể bị một vật khác trong ảnh che khuất nên trên ảnh không hiện kích thước thật của vật thể
- Vật thể bị cắt bởi rìa ảnh: "truncated" vật thể bị rìa ảnh cắt; một phần thực chất của vật thể nằm bên ngoài rìa ảnh
- Vật thể rất nhỏ/mơ hồ: nếu không đủ thông tin để chọn một trong bốn class thì ghi lại để review/hỏi giảng viên; không suy ra class chỉ vì đối tượng nằm trên đường

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_045.jpg` | Là phương tiện nhỏ, nhưng mờ, không xác định được | Thêm vào label vì đã xác định được là phương tiện | Nằm trên giao thông + là phương tiện nên có tiềm ẩn rủi ro về an toàn |
| `drive_028.jpg` | Là phương tiện lớn, nhưng mờ, không xác định được | Thêm vào label vì đã xác định được là phương tiện | Nằm trên giao thông + là phương tiện nên có tiềm ẩn rủi ro về an toàn |
| `drive_003.jpg` | Không xác định được là gì nhưng nó nằm ở trên đường giao thông (Phía xa sâu bên trái trên của ảnh | Không nên cho vào label và train | Vì như vậy model sẽ bắt cả nhiễu |


> **CẦN HỌC VIÊN RÀ SOÁT TRƯỚC KHI NỘP:** Ở `drive_045.jpg` và `drive_028.jpg`, bảng vẫn ghi *đã thêm nhãn dù không xác định được loại phương tiện*, với lý do “nằm trên đường”. Lý do này chưa phù hợp schema bốn class. Giữ nguyên ghi chép quyết định ban đầu để minh bạch, nhưng cần bạn xem lại ảnh, xác nhận loại xe (nếu có thể), sau đó cập nhật **quyết định cuối và lý do thực tế**. Đừng tự khẳng định đã sửa annotation nếu chưa sửa.
