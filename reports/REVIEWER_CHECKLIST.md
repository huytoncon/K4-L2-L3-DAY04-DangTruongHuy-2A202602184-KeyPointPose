# Reviewer checklist - điền khi kiểm bài người khác

Người gán: partner   Người kiểm: Đặng Trường Huy   Ngày: 16/9

Chạy trước khi soi bằng mắt:

```bash
python3 tools/check_pose_labels.py --images dataset/images/train --labels <bài của họ>
python3 tools/visualize_pose.py --images dataset/images/train --labels <bài của họ> --out /tmp/vis_review
python3 tools/visibility_report.py --labels dataset/labels/train --compare <bài của họ>
```

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ✅ | `check_pose_labels.py` đọc đủ 20/20 file, 28 skeleton, không có cảnh báo thiếu điểm |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ⚠️ | `train_02` (người 1) và `train_16` (người 1) bị công cụ nghi đảo trái/phải — xem chi tiết trong `review_partner.md` |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ⚠️ | `train_16` có 2 người chồng lấn (frisbee), vùng hông người 1 bị người 2 che một phần — cần soi kỹ, chưa chắc là nhầm người |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ⚠️ | 6 lượt cảnh báo ở `train_01`, `train_04` (x2), `train_10`, `train_13` — khớp chân `v=0` trong khi người còn giữa khung |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ⚠️ | Cùng nhóm lỗi với mục 4 — cần đối chiếu toạ độ gốc COCO để biết khớp nào thật sự ngoài khung |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | — | Không có công cụ tự động phát hiện; soi bằng mắt qua `outputs/vis_review/` không thấy điểm `v=2` nào rõ ràng vô lý |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | — | Chỉ nhận được `.txt` đã convert, không có file COCO JSON gốc của partner để đếm trực tiếp 51 số |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ✅ | `check_pose_labels.py` parse thành công toàn bộ 20 file, không có lỗi định dạng |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ✅ | `reports/visibility_report_partner.md` (của họ) + `reports/visibility_compare.md` (bảng so sánh thật, chạy trực tiếp trên `.txt` gốc) |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | — | Không nhận được file `GUIDELINE_MINI.md` của partner để đối chiếu |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ✅ | 0 lỗi (chỉ có 10 cảnh báo không chặn nộp) |

## Lỗi tìm được

Chép sang `reports/review_partner.md`. Mỗi dòng một lỗi, đủ bốn cột - người sửa phải
mở đúng chỗ đó được mà không cần hỏi lại.

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| train_02 | 1 | vai/hông (trái-phải) | Nghi đảo trái/phải, góc chụp từ sau lưng khó xác nhận | Hỏi lại partner, đối chiếu hướng cơ thể thật trong CVAT |
| train_16 | 1 | hông (trái-phải) | Nghi đảo trái/phải; tay đã đúng, nghi ngờ nằm ở vùng hông bị che một phần bởi người thứ 2 | Phóng to vùng hông trong CVAT, kiểm lại hai điểm hip |
| train_01, 04, 10, 13 | nhiều người | khớp phần chân (gối/mắt cá) | `v=0` khi người còn trong khung — nên là `v=1` | Đổi cờ + đặt chấm ước lượng nếu khớp còn trong khung ảnh |

Chi tiết đầy đủ: `reports/review_partner.md`.

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: dùng `v=0` (Outside) cho khớp phần chân khi người
  còn trong khung ảnh (6 lượt cảnh báo ở 4/20 ảnh).
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Guideline chưa rõ — partner áp dụng nhất
  quán trong cùng kiểu tình huống, chỉ lệch ranh giới quyết định so với luật lớp. Lỗi này cũng
  xuất hiện ở bài của người kiểm (15/20 cảnh báo tương tự), nên cả hai bên cần thống nhất lại
  trước khi sửa.
