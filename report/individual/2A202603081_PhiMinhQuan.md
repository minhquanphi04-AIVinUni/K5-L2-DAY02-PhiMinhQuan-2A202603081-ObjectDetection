# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Phí Minh Quân
- MSSV: 2A202603081
- Email: minhquanphi04@gmail.com
- Lớp: 2B - LAB - E402
- Link repo bài làm: https://github.com/minhquanphi04-AIVinUni/K5-L2-DAY02-PhiMinhQuan-2A202603081-ObjectDetection.git
- Link Colab đã chạy: https://colab.research.google.com/drive/1wzrRlHKzjIhTMbrJcvo0M9Tl6fkOL5oY (cần xác nhận người chấm có quyền xem)

## 2. Schema và mini guideline

[Mini guideline — `GUIDELINE_MINI.md`](../../GUIDELINE_MINI.md)

Bộ nhãn nộp dùng đúng schema `0=car, 1=truck, 2=bus, 3=van`. Các class phụ `LUV`, `sUV` trong bản export CVAT ban đầu đã được quy về bốn class theo quy trình remap. Việc chuyển class theo ID không thay thế kiểm tra trực quan từng xe.

## 3. Annotation và self-QC

- **Số file nhãn train:** 50 file `.txt` / 50 ảnh, đã kiểm tra trong bản repo này.
- **Kết quả checker:** `python3 tools/check_yolo_labels.py` trả về **“ĐẠT cấu trúc: 50/50 file nhãn hợp lệ”**.
- **Lỗi định dạng/ID đã xử lý:** chuyển đổi `van/bus` về đúng thứ tự class và chuẩn hóa `LUV → van`, `sUV → car`; tọa độ bounding box được giữ nguyên khi remap. Phép chuyển `LUV → van` cần được đối chiếu với từng đối tượng, không thể xem là đúng trong mọi tình huống.
- **Danh sách ít nhất 10 ảnh đã tự rà bằng mắt:** **[CẦN BẠN BỔ SUNG: điền chính xác tên 10 ảnh đã tự kiểm tra, ví dụ `drive_...jpg`; không điền ảnh chưa rà]**.
- **Các lỗi phát hiện và cách sửa:** Model pre-annotation tạo quá nhiều box; kết quả Gold QC hiện có FP cao. **[CẦN BẠN BỔ SUNG lỗi bạn thực sự tự sửa trên từng ảnh nếu đã review]**.
- **Commit khóa nhãn ban đầu:** **[CẦN BẠN BỔ SUNG commit hash trước khi nhận Gold Set, lấy bằng `git log --oneline`]**.

## 4. Gold QC và rework

[Kết quả annotation evaluation (`annotation_evaluation.json`)](../../outputs/annotation_evaluation.json)

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 537 | Chưa có kết quả mới |
| FP | 972 | — |
| FN | 116 | — |
| Precision | 0.3559 | — |
| Recall | 0.8224 | — |
| F1 | 0.4968 | — |
| mean_iou_of_matches | 0.8433 | — |
| passed | false | Chưa xác minh |

F1 0.4968 **chưa đạt** yêu cầu 0.80, trong khi mean IoU 0.8433 đã vượt ngưỡng 0.70. Bộ nhãn nộp hiện tại có 1.509 box, trong khi Gold Set có 653 box; 972 dự đoán bị tính là FP. Sự chênh lệch này cho thấy cần ưu tiên rà box thừa và class sai, nhưng không thể coi mọi FP đều là box thừa.

**Trạng thái:** Chưa có bằng chứng trong ZIP về một lần đánh giá sau rework đạt chuẩn; không ghi `passed=true` nếu chưa chạy ra kết quả mới. Nếu đã rework và chạy evaluator ở lần khác, hãy cập nhật số liệu có thật, giữ kết quả trước/sau rõ ràng.

## 5. Kết quả YOLO

- **Runtime, GPU/CPU, phiên bản Ultralytics và Torch:** Google Colab. File `assets/yolo26_runs/vehicle_annotation/args.yaml` của **lần train được đóng gói** ghi `device=0` (thiết bị GPU số 0); **[CẦN BẠN XÁC NHẬN GPU cụ thể và phiên bản Ultralytics/Torch/Python của đúng lần chạy này từ Colab]**. Không dùng thông tin CPU của lần chạy thử trước nếu không cùng lần xuất metrics.
- **Các tham số:** Theo `args.yaml`: `model=yolo26n.pt`, `epochs=50`, `imgsz=640`, `batch=-1`, `device=0`, `seed=20260909`, `plots=True`. Các thiết lập này khớp cấu hình train trong notebook gốc, với `DEVICE` được runtime lựa chọn. **[CẦN BẠN XÁC NHẬN có chỉnh tay tham số nào khác không]**.
- **Best checkpoint (chỉ ghi đường dẫn trong Colab, không commit `.pt`):** `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | **0.6360** |
| Recall | **0.3831** |
| mAP50 | **0.4313** |
| mAP50–95 | **0.2786** |

Số liệu lấy từ [`model_eval_metrics.json`](assets/model_eval_metrics.json), thống nhất với các minh chứng YOLO26 của phiên đã đóng gói.

### Minh chứng chạy model

- [10 ảnh eval đã dự đoán](assets/eval_predictions/visuals/)
- [Ảnh eval `drive_058.jpg`](assets/eval_predictions/visuals/drive_058.jpg)
- [Training results](assets/yolo26_runs/vehicle_annotation/results.png)
- [Log 50 epochs](assets/yolo26_runs/vehicle_annotation/results.csv)
- [Confusion matrix của lượt eval](assets/yolo26_runs/vehicle_eval/confusion_matrix.png)
- [Box PR curve của lượt eval](assets/yolo26_runs/vehicle_eval/BoxPR_curve.png)

**Ví dụ cụ thể:** Trong `drive_058.jpg`, một phương tiện ở gần mép phải ảnh (vùng khoảng x=577–623, y=219–289 trên ảnh 640×640) có nhãn eval là `bus` (class 2), nhưng ảnh dự đoán cho thấy mô hình gán thành `truck` với confidence xấp xỉ 0.57. Đây là ví dụ về nhầm lẫn phân loại cần đối chiếu guideline; hình tham khảo tại liên kết trên.

### 1. Lớp nào có recall thấp nhất? Chỉ ra một ảnh eval minh họa.

Ở **một lần chạy Colab trước đó**, bảng validation từng class cho thấy `truck` và `bus` cùng đạt Recall = 0.000. **Lưu ý:** con số này thuộc lượt đo trước, không đủ căn cứ coi là bảng Recall cho checkpoint của ZIP hiện tại; `model_eval_metrics.json` chỉ lưu metrics tổng. **[CẦN BẠN CHẠY/CHỤP BẢNG RECALL THEO CLASS CỦA LẦN CHẠY CUỐI ĐỂ XÁC NHẬN]**.

Ảnh minh họa có thật: [`drive_058.jpg`](assets/eval_predictions/visuals/drive_058.jpg) có một `bus` ở bên phải bị dự đoán thành `truck` (confidence xấp xỉ 0.57). Kết quả minh họa lỗi phân lớp, nhưng chưa tự chứng minh class nào có Recall thấp nhất ở toàn bộ 10 ảnh.

### 2. Nếu mAP50 cao nhưng mAP50-95 thấp, box đang gặp vấn đề gì?

mAP50 đánh giá phát hiện tại IoU = 0.50, còn mAP50–95 tính trung bình trên các ngưỡng 0.50 đến 0.95. Nếu mAP50 lớn hơn mAP50–95 đáng kể, các bounding box có thể đủ gần để vượt ngưỡng 0.50 nhưng chưa ôm sát đối tượng ở ngưỡng IoU cao hơn (box lệch, quá rộng, quá hẹp hoặc xử lý vật thể bị che/cắt chưa chính xác). Sai class, confidence và thứ tự xếp hạng dự đoán cũng ảnh hưởng đến mAP.

Trong phiên hiện tại, mAP50 = **0.4313** và mAP50–95 = **0.2786**, phản ánh chất lượng phát hiện/định vị vẫn còn hạn chế.

### 3. Có lỗi `truck`/`bus`/`van`/`car` nào liên quan tới guideline không?

Có trường hợp rõ ràng tại `drive_058.jpg`: đối tượng được gán chuẩn `bus` nhưng model dự đoán `truck`. Đây là lỗi phân biệt nhóm phương tiện có hình dạng giống nhau ở góc nhìn CCTV. Theo schema, `bus` là phương tiện chở khách dạng xe buýt; `truck` là phương tiện chở hàng. Ngoài ra, dữ liệu CVAT ban đầu có các nhãn ngoài schema (`LUV`, `sUV`); chúng đã được remap theo bốn class, nhưng cần review để tránh sai ngữ nghĩa, nhất là `LUV → van`.

### 4. Vì sao không dùng 10 ảnh eval để train dù chúng có nhãn?

10 ảnh eval được giữ riêng để validation và đánh giá mô hình trên dữ liệu không được sử dụng tối ưu trọng số khi train. Nếu trộn chúng vào train, model có thể ghi nhớ ảnh và nhãn, làm Precision/Recall/mAP trên chính tập đó lạc quan hơn thực tế (data leakage). Trong notebook này, tập eval còn tham gia validation/chọn best checkpoint, nên cũng **không** thể coi là một test set hoàn toàn độc lập.

### 5. Nhận xét tổng quan về kết quả thực nghiệm

YOLO26n đạt Precision **0.6360**, Recall **0.3831**, mAP50 **0.4313** và mAP50–95 **0.2786** trên 10 ảnh eval. Kết quả phát hiện chưa đồng đều; ảnh `drive_058.jpg` cho thấy mô hình có thể nhầm `bus` thành `truck`. Dữ liệu train hiện có F1 annotation 0.4968 theo Gold QC, chưa đạt yêu cầu đầu vào của bài nên đây chỉ là kết quả của bộ nhãn đang có, không thể coi là đã vượt checkpoint Gold QC. Cần cải thiện nhãn thừa/sai, tăng sự nhất quán trong guideline và đánh giá lại sau khi rework.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Chuẩn hóa nhãn YOLO | CVAT export 6 class; checker chỉ chấp nhận ID 0–3 | Hoán đổi `bus`/`van`; chuyển `LUV → van`, `sUV → car` trong file `.txt` | Checker đạt 50/50, cần rà lại tính đúng ngữ nghĩa của `LUV` |
| Gold QC | FP = 972, F1 = 0.4968 chưa đạt | Đã kiểm tra số lượng box và phân tích nguyên nhân có thể: box dư hoặc class sai | Chưa có kết quả sau rework đạt chuẩn |
| Import lại CVAT | CVAT báo `Could not match item id: drive_054` | Kiểm tra tên và tập ảnh trong task so với file import | Cần bảo đảm task dùng đúng 50 ảnh và tên tương ứng nếu tiếp tục rework qua CVAT |

## 7. Tự đánh giá theo rubric

- **Đã có bằng chứng:** 50 nhãn hợp lệ về cấu trúc; kết quả Gold QC ở `outputs/annotation_evaluation.json`; log train 50 epochs; 10 ảnh dự đoán eval; metrics và biểu đồ.
- **Chưa đạt:** Gold QC F1 = 0.4968 thấp hơn ngưỡng 0.80.
- **Chưa đủ minh chứng trong báo cáo:** danh sách ít nhất 10 ảnh đã tự rà, commit hash khóa nhãn, xác nhận Recall theo class của lần train cuối và thông tin runtime/version tương ứng.
- **Tự chấm điểm:** **[CẦN BẠN TỰ ĐÁNH GIÁ sau khi đối chiếu `RUBRIC.md`; không tự nhận đã đạt gate CP5]**.

## 8. Công cụ AI và tài liệu tham khảo

Đã sử dụng YOLO26 pretrained để hỗ trợ pre-annotation, sau đó dùng CVAT để review nhãn và Python để chuẩn hóa class ID. ChatGPT được dùng để giải thích lỗi, kiểm tra cấu trúc file, hướng dẫn Git/Colab và rà soát báo cáo. Kết quả checker và chỉ số model được lấy từ công cụ thực thi, không thay thế bằng số liệu tự ước lượng. Các quyết định gán nhãn cuối cùng và phần tự đánh giá học viên cần tự xác nhận.

Tài liệu: [`GUIDE.md`](../../GUIDE.md), [`SUBMISSION.md`](../../SUBMISSION.md), [`RUBRIC.md`](../../RUBRIC.md), notebook `notebooks/day2_annotation_to_yolo.ipynb` và [tài liệu Ultralytics](https://docs.ultralytics.com/).
