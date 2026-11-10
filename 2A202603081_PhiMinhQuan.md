# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Phí Minh Quân
- MSSV: 2A202603081
- Email: minhquanphi04@gmail.com
- Lớp: 2B - LAB - E402
- Link repo bài làm: https://github.com/minhquanphi04-AIVinUni/K5-L2-DAY02-PhiMinhQuan-2A202603081-ObjectDetection.git
- Link Colab đã chạy:

## 2. Schema và mini guideline 

[[GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). ](https://github.com/minhquanphi04-AIVinUni/K5-L2-DAY02-PhiMinhQuan-2A202603081-ObjectDetection/blob/6678ada2574d1eee66176e6023fa7ed8f7612084/GUIDELINE_MINI.md)
## 3. Annotation và self-QC

- Số file nhãn train:
- Kết quả checker và lỗi đã xử lý:
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework
https://github.com/minhquanphi04-AIVinUni/K5-L2-DAY02-PhiMinhQuan-2A202603081-ObjectDetection/blob/b03ca0d8c8cc3c93b5d5bdeedcf8481b0a114361/outputs/annotation_evaluation.json
| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 537 | |
| FP | 972 | |
| FN | 116 | |
| Precision | 0.3559 | |
| Recall | 0.8224 | |
| F1 | 0.4968 | |
| mean_iou_of_matches | 0.8433 | |
| passed | false | |

## 5. Kết quả YOLO

- Runtime Google Colab — CPU, GPU/CPU: Intel Xeon CPU @ 2.20GHz , phiên bản 
Ultralytics	8.4.208
Python	3.13.18
PyTorch	2.14.0+cpu
Model	YOLO26n. 
- Các tham số thay đổi so với notebook nếu có và lý do:
- Đường dẫn best checkpoint trong phiên chạy:

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | |
| Recall | |
| mAP50 | |
| mAP50–95 | |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

### 1. Lớp nào có Recall thấp nhất? Chỉ ra một ảnh eval minh họa.

Theo kết quả đánh giá YOLO26 trên 10 ảnh eval, hai lớp có Recall thấp nhất là **truck và bus, đều đạt 0.000**.

Cụ thể:
- Car: Recall = 0.883 (94 đối tượng).
- Truck: Recall = 0.000 (5 đối tượng).
- Bus: Recall = 0.000 (2 đối tượng).
- Van: Recall = 0.344 (12 đối tượng).

Điều này cho thấy mô hình chưa phát hiện đúng được các đối tượng truck và bus tại ngưỡng đánh giá Recall được báo cáo. Một nguyên nhân có thể là dữ liệu huấn luyện chưa cân bằng giữa các lớp, khiến mô hình học tốt hơn lớp car nhưng chưa đủ khả năng nhận diện các phương tiện ít xuất hiện.


### 2. Nếu mAP50 cao nhưng mAP50-95 thấp, bounding box đang gặp vấn đề gì?

mAP50 chỉ yêu cầu mức độ trùng khớp IoU từ 0.50, còn mAP50-95 tính trung bình kết quả đánh giá trên nhiều ngưỡng IoU từ 0.50 đến 0.95.

Khi mAP50 cao nhưng mAP50-95 thấp, điều đó cho thấy mô hình có thể nhận diện được đối tượng, nhưng bounding box chưa đủ chính xác khi áp dụng các ngưỡng IoU nghiêm ngặt hơn.

Những lỗi có thể gặp gồm box quá rộng, quá hẹp, bị lệch vị trí hoặc chưa ôm sát ranh giới đối tượng.

Trong thực nghiệm, mAP50 = 0.3898 và mAP50-95 = 0.2199. Khoảng cách này cho thấy chất lượng định vị bounding box còn khả năng cải thiện, mặc dù kết quả cũng chịu ảnh hưởng bởi sai sót nhận diện và phân loại.

### 3. Có lỗi truck/bus/van/car nào liên quan tới guideline không?

Có khả năng tồn tại các lỗi liên quan đến việc phân biệt những loại phương tiện có ngoại hình tương đồng.

Theo guideline, SUV và pickup được xếp vào car, xe tải chở hàng thuộc truck, xe buýt/minibus chở khách thuộc bus, còn van/minivan thuộc van.

Trong quá trình chuẩn hóa nhãn, dataset ban đầu có thêm hai class LUV và sUV. Các nhãn này đã được chuyển lần lượt về van và car để phù hợp với schema bốn class của bài tập. Việc ánh xạ theo class ID có thể tạo ra sai nhãn nếu một đối tượng ban đầu không thực sự thuộc class đích.

Kết quả đánh giá cho thấy car đạt Recall 0.883, trong khi van chỉ đạt 0.344, truck và bus đều bằng 0.000. Sự chênh lệch này là dấu hiệu cần kiểm tra lại tính cân bằng dữ liệu, cách phân loại và tính nhất quán của guideline. Tuy nhiên, chỉ từ các metrics chưa thể kết luận cụ thể có bao nhiêu trường hợp bị nhầm class.

### 4. Vì sao không dùng 10 ảnh eval để train dù chúng có nhãn?

Không sử dụng 10 ảnh eval để huấn luyện vì tập này được dành cho việc đánh giá hiệu năng và khả năng tổng quát hóa của mô hình trên dữ liệu không thuộc tập train.

Nếu sử dụng những ảnh này trong quá trình huấn luyện, mô hình có thể ghi nhớ đặc điểm và nhãn của chúng, làm kết quả Precision, Recall và mAP khi đánh giá trở nên thiếu khách quan. Đây là hiện tượng data leakage.

Trong bài thực hành, 50 ảnh train được dùng để huấn luyện YOLO26, còn 10 ảnh eval được sử dụng để validation và đánh giá theo notebook. Vì tập eval cũng tham gia lựa chọn checkpoint tốt nhất, kết quả này chưa tương đương với việc đánh giá trên một test set hoàn toàn độc lập.

### 5. Nhận xét tổng quan về kết quả thực nghiệm

Mô hình YOLO26 được huấn luyện và đánh giá với bốn class phương tiện gồm car, truck, bus và van.

Kết quả đánh giá trên 10 ảnh eval, gồm 113 đối tượng, đạt:
- Precision = 0.5708.
- Recall = 0.3068.
- mAP50 = 0.3898.
- mAP50-95 = 0.2199.

Mô hình nhận diện lớp car tương đối tốt nhưng có Recall thấp ở van và không phát hiện đúng được truck, bus tại ngưỡng đánh giá được báo cáo.

Kết quả cho thấy khả năng nhận diện chưa đồng đều giữa các lớp. Các nguyên nhân có thể bao gồm dữ liệu huấn luyện ít, mất cân bằng số lượng đối tượng theo class và sự thiếu nhất quán trong annotation.

Hướng cải thiện là tăng số ảnh huấn luyện, bổ sung các trường hợp khó của truck/bus/van, kiểm tra chất lượng nhãn và tiếp tục fine-tune mô hình với dữ liệu đa dạng hơn.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| CP1 |  | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo
Em sử dụng yolov26 cho việc annotate và sau đó check lại toàn bộ, trong quá trình làm em đã annotate quá tay dẫn đến F1 và presision thấp.
Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
