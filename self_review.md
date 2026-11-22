# Tự đánh giá — Nguyễn Tấn Dũng

## 1. Bốn cảnh cần đối chiếu trực quan (theo AutoQC và diff)

Dưới đây là **4 ca cần rà soát** từ AutoQC/diff, **chưa phải bằng chứng đã mở ảnh gốc và sửa**. Người nộp cần mở lại CVAT để chốt loại lỗi thực tế.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S01_ir.png | child_seat / infant_seat | wrong_class (nghi vấn) | Check cá nhân cảnh báo cùng rear_right có hai loại ghế; cần xác minh có trùng đối tượng | Chưa xác nhận |
| S07_rgb.png | infant_seat / child_seat | wrong_class (nghi vấn) | Diff: mine infant_seat, peer child_seat; phải xem hình dạng thực tế | Chưa xác nhận |
| D01.png | eye_left / face13 | flag (nghi vấn) | Check cá nhân: eye_left unknown nhưng không có left_eye_* occluded/outside; cần xác minh mắt có rõ không | Chưa xác nhận |
| D06.png | face13 | lr_flip (nghi vấn) | Cảnh báo điểm trái/phải theo ảnh xoay; theo GUIDE §3 mắt trái có y lớn hơn; cần kiểm tra tư thế mặt | Chưa xác nhận |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Chưa thể khẳng định lỗi lặp lại từ dữ liệu AutoQC; có nhiều cảnh báo liên quan trạng thái mắt/cờ keypoint và nhầm loại/vị trí ghế.

Vì sao mình mắc: Chưa đủ bằng chứng để khẳng định nguyên nhân; các tình huống IR, mặt xoay và ánh xạ ghế dễ nhầm, cần so với ảnh gốc.

Lần sau tránh bằng cách: Chốt frame_meta đầu tiên; đối chiếu quy ước trái/phải theo người và ghế theo xe; kiểm cờ occluded/outside trước khi xuất; rà soát RGB và IR riêng.

## 3. Kết quả QC và review chéo

- Người review đối chiếu: **Nguyễn Trọng Hùng** (`2A202603147`), [repo GitHub](https://github.com/ngtronghung004-droid/K5-L2-DAY12-NguyenTrongHung-2A202603147-DMSOMS/tree/main).
- Kết quả so sánh với bản peer: 22 ảnh chung, 156 khác biệt; QC peer báo 22 lỗi thiếu `frame_meta` và 5 cảnh báo.
- Việc đã có bản peer và chạy `diff` không đồng nghĩa reviewer đã xác nhận từng verdict. Các ca trực quan cần đối chiếu trên CVAT trước khi kết luận.


Đã có 156 điểm khác biệt. Năm dòng modality/frame_meta được quyết định theo quy tắc schema; **chưa xác định trường hợp peer đúng và mine sai** vì cần xem trực tiếp ảnh. Sau khi review thực tế, bổ sung một dòng có căn cứ vào mục này, không tự khẳng định bên nào đúng.

## 4. Đánh giá kết quả cá nhân và phạm vi xác minh

- Export chính: 22/22 ảnh có `frame_meta`, AutoQC **0 ERROR, 5 WARN**. Export mở rộng: 4/4 ảnh có `frame_meta`, **0 ERROR, 0 WARN**.
- Checkpoint A và B đã được chạy hồi cứu từ export cuối; không ghi nhận là đã hoàn thành tại thời điểm checkpoint ban đầu.
- Peer Nguyễn Trọng Hùng: 22 ảnh, **22 ERROR thiếu `frame_meta` và 5 WARN**; diff ghi nhận **156 điểm khác biệt**. Đây không đồng nghĩa 156 lỗi.
- Đã xác định 5 dòng khác biệt về `modality`/`frame_meta` bằng tên ảnh và quy tắc bắt buộc. Kết luận này chưa thay thế xác nhận của người review.
- Các cảnh báo bản cá nhân cần tiếp tục đối chiếu ảnh trước khi đánh dấu đã sửa: D01 `eye_left=unknown`; D05 hai mắt `unknown`; D06 trật tự mắt trái/phải; S01_ir cùng vị trí ghế có hai loại nhãn. Không gắn cờ occluded/outside nếu mắt chỉ mờ mà không bị che.
- Hạn chế: Hiện chỉ có báo cáo XML/AutoQC/diff và lịch sử trao đổi, chưa có bằng chứng xem lại trực tiếp toàn bộ các ca cảnh báo trên CVAT.

## 5. Quyết định QC đã chốt và phương án xử lý WARN

**E3 đã chốt 5 dòng `frame_meta.modality`:** D01.png (`ir`), D02.png (`ir`), S01_ir.png (`ir`), S01_rgb.png (`rgb`) và S02_rgb.png (`rgb`). AutoQC cho thấy bản peer không có `frame_meta` trên cả 22 ảnh, trong khi bản cá nhân đủ 22 tag. Vì vậy các dòng này chọn `verdict=mine` theo quy định bắt buộc; người được review cần bổ sung tag. Đây là kết luận QC theo dữ liệu và guideline, **không phải xác nhận Nguyễn Trọng Hùng đã đồng ý hoặc đã chỉnh sửa**.

**Phương án chốt xử lý 5 WARN cá nhân (có điều kiện):**

| Ảnh / cảnh báo | Quyết định và bước tiếp theo | Tình trạng xác minh |
|---|---|---|
| D01 — `eye_left=unknown` | Tạm giữ `unknown` nếu mắt không đủ chi tiết; chỉ đổi `open/closed` khi nhìn rõ; không bật `occluded/outside` nếu mắt chỉ mờ | Chưa xem lại ảnh |
| D05 — `eye_left=unknown` | Tạm giữ `unknown`, kiểm tra trạng thái mắt và cờ keypoint tương ứng | Chưa xem lại ảnh |
| D05 — `eye_right=unknown` | Tạm giữ `unknown`, kiểm tra trạng thái mắt và cờ keypoint tương ứng | Chưa xem lại ảnh |
| D06 — trái/phải keypoint | Giữ nguyên **nếu** đã xác nhận theo mắt trái/phải của tài xế trong ảnh xoay; nếu sai thì sửa | Chưa xem lại ảnh |
| S01_ir — `child_seat` và `infant_seat` ở `rear_right` | Cần đối chiếu từng box: nếu trùng một ghế phải bỏ nhãn sai; nếu hai đối tượng riêng thì giữ và kiểm tra lại `seat` | Chưa xem lại ảnh; chưa kết luận đúng/sai |

**Nguyên tắc:** 0 ERROR không chứng minh toàn bộ box/keypoint đúng. Chỉ ghi vào `check_final.txt` rằng một WARN “đã kiểm tra và giữ nguyên” sau khi thực tế mở ảnh đối chiếu. Nếu sửa CVAT, xuất ZIP và chạy kiểm tra lại.
