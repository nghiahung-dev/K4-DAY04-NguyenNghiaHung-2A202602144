# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Nghĩa Hùng   Nhóm: T013   Ngày: 16/09/2026


## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 342 / 105 / 29 |
| Thời gian trung bình mỗi ảnh | 5 phút/ảnh |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear: 57.14%
2. right_ear: 42.86%
3. right_wrist: 32.14%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Không hoàn toàn; các khớp có tỉ lệ `%v=1` cao nhất chỉ là những vị trí **hay bị che khuất bề mặt** nhưng lại rất dễ suy ra tọa độ giải phẫu, ví dụ `left_ear`/`right_ear` bị tóc che vẫn dóng được theo trục ngang qua sống mũi và đuôi mắt, còn `right_wrist` bị khay bánh che vẫn nội suy chuẩn từ cẳng tay. Những khớp thực sự **khó xác định vị trí giải phẫu nhất** là `left_hip` và `right_hip` ở các ảnh nhân vật mặc tạp dề trùm dài qua hông hoặc ngồi sau bàn ăn, bởi nếp gấp vải rộng làm mất hoàn toàn mốc gồ xương chậu, khiến việc định vị tâm khớp phải phán đoán mò thay vì có điểm tựa thị giác rõ ràng.
## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.921 | 0.954 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.966 | 1.000 |
| Lỗi dao_trai_phai | 0 | 0 |
| Lỗi nham_nguoi | 0 | 0 |
| Lỗi xoa_khop_bi_che | 0 | 0 |

Tôi đã sửa gì giữa hai lần chạy (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

- **train_13.jpg (người thứ 1):** Bổ sung hoàn chỉnh 1 skeleton bị thiếu gồm đủ 17 keypoints (trước đó OKS = 0.000 do sót người).
- **train_13.jpg & train_14.jpg:** Nắn chỉnh lại 8 khớp bị lệch nhẹ (chủ yếu là `left_shoulder`, `right_shoulder`, và `left_knee`) để kéo tâm khớp về đúng vị trí giải phẫu chuẩn hơn thay vì lệch theo viền quần áo.

Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào? Ảnh đó dễ hay khó? Nếu là ảnh dễ, bạn nghĩ vì sao mình vẫn sai?

Tôi không gặp lỗi đảo trái/phải trong toàn bộ tập gán (số lỗi `dao_trai_phai: 0`). Tôi đã tuân thủ đúng quy ước xác định trái/phải theo góc nhìn giải phẫu của chính người trong ảnh (ngược lại với hướng nhìn của mắt người gán từ màn hình), nhờ vậy tránh được lỗi nhầm lẫn đối xứng phổ biến này ngay từ lần gán đầu tiên.

## 3. Kiểm chéo

Bạn cùng nhóm: ______

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| | | | | |
| | | | | |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

-

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   `pose_mAP50-95` **tăng +0.0055** (từ `0.6853` lên `0.6908`) và `pose_precision` tăng **+0.0058**. Dù số lượng tập train rất nhỏ (20 ảnh), model đã bắt đầu học được quy luật ước lượng và gán cờ `v=1` cho các khớp bị che (như tai lấp sau tóc, cổ tay khuất sau đồ vật) theo đúng quy ước bài lab thay vì gắn cờ `v=0` như COCO gốc; tuy nhiên, `box_mAP50-95` giảm nhẹ `-0.0078` do dữ liệu quá ít làm mô hình hơi bị overfit nhẹ vào kiểu bounding box hẹp của 20 ảnh này.

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   `box_mAP50-95` (`0.8041`) cao hơn `pose_mAP50-95` (`0.6908`) là **0.1133** (chênh lệch 11.33%). **Model tìm người dễ hơn tìm khớp.** Lý do vì người là một vùng không gian (bounding box) lớn với nhiều đặc trưng ngữ cảnh rõ rệt (quần áo, đầu, thân), trong khi khớp là tọa độ điểm cực nhỏ (pixel-level) rất nhạy cảm với sai số vị trí và dễ mất dấu khi bị che khuất hoặc khi cơ thể gập xoay phức tạp.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   Tại ảnh **`test_06`**, mô hình gặp lỗi **"nhầm người"** và **"trượt hẳn"**: do người phía sau bị người phía trước che khuất một phần cơ thể, mô hình đã nối nhầm các khớp của người sau sang người trước và làm trượt hoàn toàn cụm khớp chân vào nền ảnh.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   Ảnh **`train_06`** có OKS thấp nhất (**0.586**). Ở ảnh này, **nhãn tự gán đúng, model sai**. Bằng chứng dựa trên việc đối chiếu với nhãn `gold` (tập nhãn đã qua kiểm tra đạt OKS trung bình > 0.92) và kiểm tra thị giác bằng `tools/visualize_pose.py`: tư thế người trong `train_06` bị khuất góc khó, nhãn gán thủ công vẫn định vị chuẩn theo giải phẫu xương, trong khi model bị trượt vị trí khớp do chưa học đủ dạng tư thế biến thể này.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   Có, tiêu biểu là ảnh **`train_16`** (nhãn thủ công dễ gây nhầm lẫn trái/phải và model cũng chỉ đạt OKS khoảng `0.81`). Điều này cho thấy đây là một **ca biên khó (edge case)**: góc máy chụp nhân vật ở tư thế xoay vặn hoặc quay lưng nhưng ngoảnh đầu, gây xung đột giữa trục nhìn của khuôn mặt và trục cơ thể, khiến cả người gán lẫn mạng nơ-ron đều lúng túng khi phân định ranh giới giải phẫu.

## 5. Một rule evidence bạn đã dùng

- **Ảnh:** `train_03.jpg`
- **Người:** Người nam phía trước đang đứng cạnh xe đạp (PERSON 73)
- **Khớp:** `right_ankle` (cổ chân phải)
- **Quyết định trạng thái:** Chọn `v=1` (Occluded), không chọn `v=0` (Outside).

**Bằng chứng nhìn thấy và lý do chọn:**
Tại ảnh `train_03.jpg`, phần cổ chân phải của người đứng trước bị nan hoa và bánh sau chiếc xe đạp che khuất một phần, đồng thời có một con búp bê nằm ngay sát dưới mặt đất. Dù mắt không nhìn thấy trọn vẹn điểm tâm khớp xương, ta vẫn quan sát rõ toàn bộ cẳng chân quần bò xanh kéo thẳng xuống và bàn chân đặt vững trên mặt đường bên trong khung hình. Do điểm giải phẫu này vẫn hoàn toàn nằm trong khung ảnh chứ không hề bị cắt lọt ra ngoài mép, theo đúng quy tắc của bài, vị trí này bắt buộc phải chấm điểm ước lượng và kích hoạt cờ `Occluded` (`v=1`) thay vì bỏ qua hay đánh dấu `Outside` (`v=0`).