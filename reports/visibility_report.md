# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 15.96 khớp có v > 0 mỗi người
- Tổng: v=2 342 | v=1 105 | v=0 29

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 25 | 3 | 0 | 11% |
| 1 | left_eye | 20 | 8 | 0 | 29% |
| 2 | right_eye | 23 | 5 | 0 | 18% |
| 3 | left_ear | 12 | 16 | 0 | 57% |
| 4 | right_ear | 16 | 12 | 0 | 43% |
| 5 | left_shoulder | 28 | 0 | 0 | 0% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 22 | 6 | 0 | 21% |
| 8 | right_elbow | 25 | 3 | 0 | 11% |
| 9 | left_wrist | 20 | 8 | 0 | 29% |
| 10 | right_wrist | 18 | 9 | 1 | 32% |
| 11 | left_hip | 19 | 8 | 1 | 29% |
| 12 | right_hip | 22 | 5 | 1 | 18% |
| 13 | left_knee | 18 | 6 | 4 | 21% |
| 14 | right_knee | 20 | 4 | 4 | 14% |
| 15 | left_ankle | 14 | 5 | 9 | 18% |
| 16 | right_ankle | 13 | 6 | 9 | 21% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
