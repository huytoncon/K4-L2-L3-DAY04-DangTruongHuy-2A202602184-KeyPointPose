# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 13.5 khớp có v > 0 mỗi người
- Tổng: v=2 330 | v=1 48 | v=0 98

So sánh với `C:\Users\ASUS\Downloads\train-partner` (28 skeleton).
Cột **lệch** là hiệu số phần trăm v=1 - chỗ nào lệch nhiều nhất là chỗ guideline chưa nói rõ.

| # | Khớp | %v=1 (bạn) | %v=1 (đối chiếu) | lệch |
| ---: | --- | ---: | ---: | ---: |
| 3 | left_ear | 18% | 50% | 32 |
| 4 | right_ear | 11% | 36% | 25 |
| 9 | left_wrist | 14% | 36% | 21 |
| 0 | nose | 0% | 21% | 21 |
| 1 | left_eye | 4% | 25% | 21 |
| 2 | right_eye | 0% | 21% | 21 |
| 16 | right_ankle | 4% | 21% | 18 |
| 14 | right_knee | 7% | 21% | 14 |
| 10 | right_wrist | 18% | 29% | 11 |
| 13 | left_knee | 4% | 14% | 11 |
| 7 | left_elbow | 7% | 14% | 7 |
| 11 | left_hip | 25% | 18% | 7 |
| 8 | right_elbow | 14% | 11% | 4 |
| 12 | right_hip | 25% | 29% | 4 |
| 5 | left_shoulder | 11% | 7% | 4 |
| 15 | left_ankle | 7% | 11% | 4 |
| 6 | right_shoulder | 4% | 4% | 0 |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
