# Mini guideline - nhóm: Chưa cung cấp  |  người gán: Nguyễn Nghĩa Hùng  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Nếu vị trí hông còn trong ảnh nhưng bị áo che, ước lượng từ trục thân và gán `v=1`; chỉ dùng `v=2` khi mốc hông nhìn thấy rõ. Ảnh mẫu: [`train_15`](dataset/images/train/train_15.jpg). | Áo dài che bề mặt khớp nhưng không làm khớp ra ngoài ảnh. |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Phần tai nhìn rõ đủ xác định tâm thì `v=2`; nếu phải suy ra qua tóc/mũ thì `v=1`. Ảnh mẫu: [`train_04`](dataset/images/train/train_04.jpg). | Cờ phản ánh khả năng quan sát điểm giải phẫu, không phản ánh việc biết đại khái tai nằm ở đâu. |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Khớp nằm ngoài biên ảnh dùng `v=0`; khớp vẫn trong ảnh nhưng bị vật/người khác che dùng `v=1`. Ảnh mẫu: [`train_04`](dataset/images/train/train_04.jpg). | Phân biệt “không tồn tại trong ảnh” với “tồn tại nhưng không nhìn thấy”. |
| Cổ tay nằm sau tay lái / sau thân mình | Đặt điểm theo hướng cẳng tay và gán `v=1` nếu cổ tay vẫn trong khung. Ảnh mẫu: [`train_06`](dataset/images/train/train_06.jpg). | Tay lái hoặc thân người là vật che; không phải mép ảnh. |
| Hai người chồng lên nhau | Tạo skeleton riêng cho từng người; điểm bị người kia che vẫn thuộc đúng cơ thể và dùng `v=1`. Ảnh mẫu: [`train_13`](dataset/images/train/train_13.jpg). | Tránh nối nhầm khớp giữa hai người và tránh làm thiếu người. |
| Người nhỏ đến mức nào thì không gán nữa | Mọi người còn nhận diện được trong ảnh đều phải có skeleton; dùng visibility cho các khớp không đủ rõ, không tự đặt ngưỡng bỏ người. Ảnh mẫu: người thứ 3 trong [`train_13`](dataset/images/train/train_13.jpg). | Gold có 29 người; bỏ người nhỏ từng làm thiếu một skeleton và giảm OKS@0.75. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_06`, người thứ `1`, khớp `right_wrist`

- Mơ hồ ở chỗ nào: Cổ tay bị xe máy và thân người che, chỉ thấy được hướng cánh tay.
- Bạn quyết thế nào: Đặt điểm ước lượng theo đường vai–khuỷu tay và dùng `v=1`.
- Vì sao: Vị trí vẫn nằm trong khung; đây là che khuất chứ không phải ra ngoài ảnh.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Dùng `v=0` sẽ loại mất mẫu cổ tay bị che và làm model kém ở tư thế lái xe.

### Ca 2 - ảnh `train_13`, người thứ `3`, khớp `nose`

- Mơ hồ ở chỗ nào: Người ở mép trái rất nhỏ và khuôn mặt có ít pixel, ban đầu đã bị bỏ sót cả skeleton.
- Bạn quyết thế nào: Bổ sung đủ skeleton; `nose` không nhìn rõ nên đặt theo tâm mặt và dùng `v=1`.
- Vì sao: Người vẫn nhận diện được và nằm trong ảnh, do đó không được bỏ chỉ vì kích thước nhỏ.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Model học bỏ qua người nhỏ và giảm recall trong cảnh đông người.

### Ca 3 - ảnh `train_15`, người thứ `2`, khớp `left_hip`

- Mơ hồ ở chỗ nào: Áo khoác dài che đường hông, không nhìn thấy trực tiếp tâm khớp.
- Bạn quyết thế nào: Ước lượng từ trục vai, thân và chân rồi dùng `v=1`.
- Vì sao: Hông vẫn nằm trong khung và chỉ bị trang phục che.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Đánh `v=2` làm model tin một tọa độ ước lượng là quan sát chắc chắn; đánh `v=0` lại loại bỏ hoàn toàn mẫu che khuất.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: **chưa có dữ liệu của bạn cùng nhóm** (bạn: chưa xác định / họ: chưa xác định).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: chưa thể kết luận khi chưa có bảng so sánh.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: chờ kết quả kiểm chéo; không tự tạo số liệu.