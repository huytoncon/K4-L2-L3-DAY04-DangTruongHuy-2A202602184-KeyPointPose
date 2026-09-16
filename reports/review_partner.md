# Review bài partner

Người gán: partner (folder `train-partner`, nhận qua tải xuống)
Người kiểm: Đặng Trường Huy
Ngày: 16/9

Nguồn: 20 file `.txt` YOLO Pose gốc của partner (không phải bản báo cáo tổng hợp).
Đã chạy `check_pose_labels.py`, `visualize_pose.py`, `visibility_report.py --compare` trên
đúng thư mục này.

## Lỗi tìm được

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_02 | 1 | left_shoulder/right_shoulder, left_hip/right_hip | `check_pose_labels.py` nghi đảo trái/phải (heuristic dựa theo vị trí hai mắt). Xem ảnh phóng to: người quay lưng lại camera, góc mặt không đủ rõ để tự kết luận | Mở lại trong CVAT, xác nhận hướng cơ thể thật rồi đối chiếu gán trái/phải; đây là ca nên hỏi trực tiếp partner trước khi đổi |
| train_16 | 1 (áo đỏ số 73) | left_hip/right_hip | Cảnh báo đảo trái/phải, nhưng soi ảnh phóng to thì hai tay đã gán đúng (nhìn từ sau lưng, trái/phải không đảo so với ảnh). Nghi ngờ chính là vùng hông — bị người chơi thứ 2 che một phần nên khó xác định | Mở CVAT, phóng to vùng hông, kiểm lại hai điểm left_hip/right_hip riêng (không cần đổi tay) |
| train_01 | 1, 2 | left_knee, right_knee, left_ankle, right_ankle | `v=0` trong khi cả người nằm gọn giữa ảnh — nghi dùng Outside thay vì Occluded | Đổi sang `v=1` (Occluded) + đặt chấm ước lượng nếu khớp còn trong khung |
| train_04 | 1, 2 | các khớp phần chân (gối/mắt cá, tuỳ người) | Cùng lỗi `v=0` khi người còn trong khung | Kiểm từng khớp: nếu toạ độ gốc nằm trong kích thước ảnh thì đổi sang `v=1` |
| train_10 | 1 | khớp phần chân | Cùng lỗi `v=0` khi người còn trong khung | Như trên |
| train_13 | 1 | khớp phần chân | Cùng lỗi `v=0` khi người còn trong khung | Như trên |

## Reviewer checklist đã điền

Xem `reports/REVIEWER_CHECKLIST.md`.

## Hai câu kết luận

- **Lỗi lặp đi lặp lại nhiều nhất của bài này:** dùng `v=0` (Outside) cho khớp phần chân
  (gối/mắt cá) khi người vẫn còn nằm trong khung ảnh — xuất hiện ở 4/20 ảnh (6 lượt cảnh báo).
- **Lỗi thao tác hay lỗi guideline chưa rõ?** Đây là **lỗi guideline chưa rõ**, không phải lỗi
  thao tác: partner áp dụng Outside/Occluded nhất quán trong cùng một kiểu tình huống, chỉ khác
  ranh giới quyết định so với luật lớp. Same pattern cũng xuất hiện ở bài của chính mình (15/20
  cảnh báo tương tự) — cả hai bên nên thống nhất lại quy tắc "còn trong khung là v=1" trước khi
  sửa nhãn.
