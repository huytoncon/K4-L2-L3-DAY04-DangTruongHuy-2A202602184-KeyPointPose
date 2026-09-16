# Mini guideline - nhóm: <2A202602184><2A202602221>  |  người gán: Đặng Trường Huy  |  ngày: 16/9

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
| Hông của người mặc quần áo dài | `v = 1`, đặt chấm ở vị trí ước lượng giải phẫu (giao điểm xương chậu) | Hông gần như không bao giờ thấy được qua quần áo, nhưng vẫn còn trong khung -> không phải `v = 0` |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | `v = 1`, ước lượng theo hình dạng đầu, **không** dùng `v = 0` | Bị vật/tóc che là "occluded" chứ không phải "ra khỏi khung" - hai khái niệm khác nhau |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Các khớp chi dưới (gối, mắt cá) không nằm trong ảnh -> `v = 0`, không đặt chấm. Hông vẫn `v = 1`/`v = 2` nếu thân trên còn hiển thị | Đúng định nghĩa: chỉ khớp thật sự ra ngoài mép ảnh mới là `v = 0` |
| Cổ tay nằm sau tay lái / sau thân mình | `v = 1`, đặt chấm ước lượng theo hướng cánh tay | Vẫn còn trong khung, chỉ bị vật/cơ thể khác che |
| Hai người chồng lên nhau | Mỗi người vẫn đủ 17 điểm riêng; khớp bị người kia che -> `v = 1` ở đúng vị trí ước lượng của người đó, không gán nhầm sang khung người kia | Tránh lỗi "nhầm người" nêu ở chặng 3 |
| Người nhỏ đến mức nào thì không gán nữa | Nếu ở zoom 100% không phân biệt được từng khớp bằng mắt thường (ước lượng bbox cao dưới ~20-25px) thì bỏ qua, ghi chú lại | Đặt chấm đoán mò còn hại hơn là bỏ qua có ghi chú |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

**Hông của người mặc quần áo dài** - hai người mặc jean, không thấy được khớp hông qua vải:

![hong bi quan che](outputs/vis_train/train_03.jpg)

**Tai bị mũ bảo hiểm che hoàn toàn** - cả hai người đội mũ fullface, tai hoàn toàn không thấy:

![tai bi mu che](outputs/vis_train/train_04.jpg)

**Người bị cắt ở mép ảnh + cổ tay sau tay lái** - chỉ thấy từ hông trở lên, hai tay đang nắm
tay lái xe máy nên cổ tay bị chính thân xe che:

![cat mep anh va co tay sau tay lai](outputs/vis_train/train_10.jpg)

**Hai người chồng lên nhau** - hai người đứng sát nhau, bounding box giao nhau lớn (IoU ~0.44):

![hai nguoi chong len nhau](outputs/vis_train/train_03.jpg)

**Người nhỏ đến mức không gán nữa** - người mặc áo xanh phía xa bên trái không có skeleton nào
được vẽ vì quá nhỏ/mờ để gán tin cậy:

![nguoi nho khong gan](outputs/vis_train/train_13.jpg)
## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `20`, người thứ `1`, khớp `left knee, left ankle`

- Mơ hồ ở chỗ nào:bị khuất đầu gối và mắt cá sau xe
- Bạn quyết thế nào: loại bỏ(o)
- Vì sao:vì không phán đoán được vị trí
- Nếu người khác quyết ngược lại thì model học sai cái gì: sai cái chân trái

### Ca 2 - ảnh `2`, người thứ `1`, khớp `tai, mắt ,mũi`

- Mơ hồ ở chỗ nào:người này quay đi không nhìn thấy j trên khuân mặt
- Bạn quyết thế nào:bỏ hết
- Vì sao: vì không phán đoán được vị trí
- Nếu người khác quyết ngược lại thì model học sai cái gì: sai vị trí khuân mặt

### Ca 3 - ảnh `15`, người thứ `1`, khớp `left hip, knee, ankle`

- Mơ hồ ở chỗ nào: bị khuất sau cái xe máy
- Bạn quyết thế nào: đặt là occluded
- Vì sao: vì vẫn đoán được vị trí
- Nếu người khác quyết ngược lại thì model học sai cái gì: thì sẽ tạo ra chân ma

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `18%` / họ `50%`), theo sau là `right_ear` (11%/36%) và nhóm mặt/cổ tay (`nose`, `left_eye`, `right_eye`, `left_wrist`, đều lệch 21 điểm %).
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: guideline chưa rõ cho ca "tai bị tóc/mũ che một phần" và "mặt quay nghiêng chỉ thấy nửa mắt/mũi" — partner có xu hướng gán `v=1` (occluded) cho các ca này còn mình gán `v=0` (outside) hoặc `v=2`. Ngoài ra tổng `v0_outside` của mình cao bất thường so với partner (98/476 so với 33/476) và trùng với 15 cảnh báo của `check_pose_labels.py`, nên nhiều khả năng phần lớn lệch này đến từ việc **mình đang dùng Outside quá tay** ở các khớp còn trong khung, không đơn thuần là khác guideline.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: *(điền sau khi trao đổi trực tiếp với partner để chốt luật chung cho tai bị che và mặt nghiêng)*
