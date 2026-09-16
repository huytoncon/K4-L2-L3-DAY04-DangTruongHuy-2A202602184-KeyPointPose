# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Đặng Trường Huy   Nhóm: 01   Ngày: 16/9

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 28 |
| v=2 / v=1 / v=0 | 330 / 48 / 98 |
| Thời gian trung bình mỗi ảnh | *(điền theo thời gian thực tế bạn đã gán)* |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_hip - 25%
2. right_hip - 25%
3. left_ear và right_wrist - đồng hạng 18%

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

<!-- Trả lời 2–4 câu. Phân biệt "hay bị che" với "khó xác định vị trí giải phẫu"; nêu bằng
chứng nhìn thấy thay vì chỉ nêu cảm giác. -->

*(Điền câu trả lời của bạn ở đây - ví dụ: hông đúng là khó nhất vì gần như không nhìn thấy
được trên người mặc quần áo, phải ước lượng theo giải phẫu; còn tai/cổ tay thường bị tóc,
mũ bảo hiểm hoặc thân người che một phần.)*

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.867 | 0.890 |
| OKS@0.50 | 0.931 | 1.000 |
| OKS@0.75 | 0.862 | 0.931 |
| Lỗi `dao_trai_phai` | 1 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 9 | 9 |

Ghi chú thêm (không có sẵn hàng riêng trong bảng trên nhưng cần sửa cùng đợt):
- Thiếu hẳn một người: 1 -> **0** (đã gán bổ sung ở `train_13`)
- Thiếu khớp gold nhìn thấy rõ (`thieu_khop`, gold v=2 mà bài v=0): 10 -> 10 (chưa sửa)
- Trượt hẳn: 2 -> **0** (hết cùng lúc với lỗi đảo trái/phải ở `train_14`)
- Lệch nhẹ: 13 -> **3** (giảm mạnh, phần lớn đến từ chính skeleton bị đảo trái/phải cũ)

**Cổng qua bài (RUBRIC.md): OKS trung bình >=0.75 và OKS@0.75 >=0.70 và không còn lỗi `dao_trai_phai`
=> ĐÃ QUA CỔNG sau rework này** (0.890 / 0.931 / 0 lỗi đảo trái-phải).

**Danh sách việc còn lại** (không chặn cổng nữa, nhưng vẫn ảnh hưởng điểm "Cờ visibility" 15đ
và điểm chính xác vị trí - nên làm nếu còn thời gian):

| Ưu tiên | Ảnh | Người thứ | Khớp | Lỗi | Đã sửa? |
| ---: | --- | ---: | --- | --- | :---: |
| 4 | train_03 | 2 | right_wrist | Xoá khớp bị che | ☐ |
| 4 | train_04 | 2 | left_wrist, left_ear, right_ear | Xoá khớp bị che + Thiếu khớp | ☐ |
| 4 | train_10 | 1 | left_hip, right_hip | Xoá khớp bị che | ☐ |
| 4 | train_11 | 1 | right_wrist | Xoá khớp bị che | ☐ |
| 4 | train_12 | 1 | left_knee, left_ankle, right_ear | Xoá khớp bị che + Thiếu khớp | ☐ |
| 4 | train_13 | 2 | left_knee | Xoá khớp bị che | ☐ |
| 4 | train_19 | 1 | left_ear, right_ear | Xoá khớp bị che + Thiếu khớp | ☐ |
| 4 | train_02 | 1 | left_ear | Thiếu khớp (gold thấy rõ) | ☐ |
| 4 | train_06 | 1 | left_ear, right_ear | Thiếu khớp (gold thấy rõ) | ☐ |
| 4 | train_15 | 2 | right_eye, right_ear | Thiếu khớp (gold thấy rõ) | ☐ |
| 4 | train_20 | 1 | right_ear | Thiếu khớp (gold thấy rõ) | ☐ |
| 6 | train_01 | 1 | left_wrist | Lệch nhẹ | ☐ |
| 6 | train_13 | 3 | left_ankle | Lệch nhẹ | ☐ |
| 6 | train_20 | 1 | right_wrist | Lệch nhẹ | ☐ |

**Đã hoàn thành (không cần làm lại):**

| Ưu tiên | Ảnh | Người thứ | Khớp | Lỗi | Đã sửa? |
| ---: | --- | ---: | --- | --- | :---: |
| 1 | train_14 | 2 | toàn bộ cặp trái/phải | Đảo trái/phải | ✅ |
| 3 | train_13 | 1 (gold) | cả người | Thiếu hẳn một người | ✅ |
| 5 | train_14 | 2 | left_elbow, right_elbow | Trượt hẳn (80px) | ✅ (hết cùng lỗi #1) |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết "đã sửa
lại một số lỗi". -->

- `train_14.jpg`, người #2: đổi lại toàn bộ cặp khớp trái/phải (đảo trái/phải) - kéo theo
  hết luôn 2 lỗi "Trượt hẳn" (left_elbow, right_elbow) và phần lớn 10/13 lỗi "Lệch nhẹ" của
  chính skeleton này, vì các khớp trước đó lệch là do gán nhầm bên chứ không phải lệch vị trí.
- `train_13.jpg`, người #1 (theo thứ tự gold): gán bổ sung cả skeleton còn thiếu (17 điểm).

*(Nếu tiếp tục sửa 9 lỗi "Xoá khớp bị che" và 10 lỗi "Thiếu khớp" còn lại trong bảng trên,
ghi thêm từng dòng vào đây theo đúng format: ảnh + người thứ mấy + khớp + thao tác.)*

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

<!-- Nếu không có lỗi, ghi rõ "Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh." -->

Xảy ra ở `train_14.jpg`, người #2 (OKS chỉ 0.294 trước rework, thấp nhất trong 20 ảnh).
*(Điền tiếp: ảnh này dễ hay khó, và vì sao lại sai - xem `outputs/vis_train/train_14.jpg`
để nhận xét cụ thể tư thế người này.)*

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
| pose_mAP50 | | | |
| pose_mAP50-95 | | | |
| pose_precision | | | |
| pose_recall | | | |
| box_mAP50-95 | | | |

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->
