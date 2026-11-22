# Danh sách lớp v0 — Nguyễn Tấn Dũng

**Thông tin đối chiếu QC:** Nguyễn Trọng Hùng (`2A202603147`), [repo GitHub](https://github.com/ngtronghung004-droid/K5-L2-DAY12-NguyenTrongHung-2A202603147-DMSOMS/tree/main). Danh sách lớp là phần đề xuất cá nhân của Nguyễn Tấn Dũng, không phải nội dung xác nhận bởi reviewer.

Mỗi lớp/thuộc tính dưới đây phục vụ một đầu ra của model DMS hoặc OMS. Tính khả kiến trên IR chỉ là khả năng chung, không phải khẳng định cho từng frame.

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person | driver | Xác định người lái và vùng cơ thể để phân tích hành vi | Có, theo hình dạng |
| 2 | face | box | Định vị khuôn mặt để đánh giá chú ý, mắt, kính và hướng nhìn | Có, nếu mặt hiện rõ |
| 3 | eye_state | open / closed / unknown | Nhận biết trạng thái mắt theo từng khung, phục vụ đánh giá mất tập trung hoặc buồn ngủ theo chuỗi thời gian | Tùy che khuất/phản xạ |
| 4 | gaze_zone | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phân biệt nhìn đường với nhìn các vùng trong xe | Có điều kiện, theo mắt và đầu |
| 5 | face13 | 13 keypoint + occluded/outside | Ước lượng cấu trúc mặt và hỗ trợ phân tích hướng đầu/mắt | Có nếu mặt đủ lớn và điểm nhìn rõ |
| 6 | hand | driver/passenger, left/right, on_wheel | Theo dõi bàn tay và tương tác với vô lăng | Có nếu tay xuất hiện |
| 7 | phone | held / mounted / lap | Phát hiện việc tài xế tương tác với điện thoại | Có nếu thấy hình dạng điện thoại |
| 8 | seatbelt | worn_correct / worn_incorrect / not_worn / unknown | Kiểm tra trạng thái sử dụng dây an toàn | Có điều kiện; vùng tối có thể unknown |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person | rear_left / rear_middle / rear_right | Xác định người lớn và vị trí ngồi trên hàng ghế sau | Có theo hình dạng |
| 2 | face | box | Xác nhận vùng mặt người lớn khi mặt nhìn thấy | Có nếu không bị che |
| 3 | child_seat | occupied: no / unknown | Nhận diện ghế trẻ em trống, hỗ trợ hiểu tình trạng ghế | Có theo hình dạng |
| 4 | infant_seat | facing: rear / front | Phân biệt nôi xách và hướng lắp ghế em bé | Có nếu nhìn rõ cấu trúc |
| 5 | object | bag / box / cup / other | Phân biệt đồ vật với người hoặc ghế trẻ em | Có theo hình dạng; không suy màu |
| 6 | seat | rear_left / rear_middle / rear_right | Gắn đối tượng với đúng chỗ ngồi theo hướng xe | Có nếu xác định được bố cục |
| 7 | frame_meta | rgb / ir; day / night / unknown | Quản lý nguồn ảnh, tránh trộn nhãn thiếu căn cứ giữa các modality | Đây là metadata, không phải đối tượng nhìn thấy |

## 2 câu hỏi leo thang

1. Câu hỏi: Khi một vùng có hình dạng gần giống cả `child_seat` lẫn `infant_seat`, tiêu chí hình thái nào được ưu tiên để tránh gán trùng?
   Khung ví dụ: `S01_ir.png` (QC peer báo hai box child_seat cùng rear_right; bài cá nhân và peer khác lớp ở một số vùng).
   Bạn tạm gán thế nào và vì sao: Giữ nguyên nhãn cá nhân để chờ đối chiếu ảnh và coach, không tự thêm/xóa box vì chỉ có kết quả XML/QC.
2. Câu hỏi: Nếu ảnh NIR không phân biệt được mắt mở/nhắm do mờ hoặc phản xạ nhưng điểm mắt không thực sự bị che, nên thống nhất `unknown` với cờ keypoint thế nào?
   Khung ví dụ: `D05.png` (AutoQC báo trạng thái mắt unknown và cờ không nhất quán).
   Bạn tạm gán thế nào và vì sao: Giữ `unknown` khi không đủ bằng chứng; không tự gắn cờ `occluded` nếu không có che khuất thật; hỏi coach về quy tắc QC.
