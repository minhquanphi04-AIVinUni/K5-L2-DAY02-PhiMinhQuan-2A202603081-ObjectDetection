# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | xe trọng tải từ 2 đến 7 người, có cửa mở ở 2 bên cánh | xe con, sedan, SUV, taxi, xe bán tải |
| `truck` | xe trọng tải lớn đến rất lớn để chở hàng, đồ, vật liệu, có thùng xe hở ở phía sau và mở từ đằng sau | xe tải chở hàng, xe đầu kéo, xe container |
| `bus` | xe có trọng tải lớn, dài và dùng để chở người | xe bus công cộng, xe khách du lịch, xe bus 2 tầng |
| `van` | xe có trọng tải vừa, có thể chở hàng hoặc người; có dáng hình hộp liền, kín, có phân tách giữa buồng lái và khoang sau; thường có cửa trượt to để mở khoang sau | mini van, xe van chở hàng |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: toàn bộ pixel của vật thể nằm gọn trong box, viền box tiếp xúc sát với pixel ngoài cùng của vật thể
- Vật thể bị che khuất: "occluded" vật thể bị một vật khác trong ảnh che khuất nên trên ảnh không hiện kích thước thật của vật thể
- Vật thể bị cắt bởi rìa ảnh: "truncated" vật thể bị rìa ảnh cắt; một phần thực chất của vật thể nằm bên ngoài rìa ảnh
- Vật thể rất nhỏ/mơ hồ: vật thể quá nhỏ, vỡ nét hoặc không thể được xác định là class gì (Tình huống số 3)

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_045.jpg` | Là phương tiện nhỏ, nhưng mờ, không xác định được | Thêm vào label vì đã xác định được là phương tiện | Nằm trên giao thông + là phương tiện nên có tiềm ẩn rủi ro về an toàn |
| `drive_028.jpg` | Là phương tiện lớn, nhưng mờ, không xác định được | Thêm vào label vì đã xác định được là phương tiện | Nằm trên giao thông + là phương tiện nên có tiềm ẩn rủi ro về an toàn |
| `drive_003.jpg` | Không xác định được là gì nhưng nó nằm ở trên đường giao thông (Phía xa sâu bên trái trên của ảnh | Không nên cho vào label và train | Vì như vậy model sẽ bắt cả nhiễu |
