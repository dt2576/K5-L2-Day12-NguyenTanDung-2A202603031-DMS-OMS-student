# Khai báo dùng AI — Nguyễn Tấn Dũng

Trong các trao đổi được cung cấp ở đây, chỉ sử dụng dữ liệu xuất annotation, kết quả AutoQC/diff và nội dung hướng dẫn; không có bằng chứng đã đưa ảnh cabin gốc vào AI. Đã trao đổi dữ liệu xuất annotation và kết quả text/QC với ChatGPT; cần tuân thủ chính sách dữ liệu của mentor khi nộp/chia sẻ.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | ChatGPT | Giải thích RGB, IR-sim, NIR, frame_meta, light và cách tạo tag | Hỗ trợ hiểu metadata của 22 ảnh | Đối chiếu GUIDE.md §§3–4, chạy cabin_check |
| 2 | ChatGPT | Phân biệt person, face, face13, glasses, gaze_zone; quy ước mắt/tay | Hỗ trợ hiểu cách gán DMS | Đối chiếu GUIDE.md §§4.1, 4.5–4.7; còn WARN cần rà soát trực quan |
| 3 | ChatGPT | Phân biệt OMS child_seat, infant_seat, object, seat, occupied, facing | Hỗ trợ chọn nhãn/thuộc tính theo guideline | Đối chiếu GUIDE.md §§4.2, 4.8; chưa xác minh mọi ca tranh chấp |
| 4 | ChatGPT | Hướng dẫn xuất CVAT, chạy check/diff, đọc ERROR/WARN, đóng gói | Chạy lệnh trên WSL, tạo check_final/check_extra/checkpoint và disagreement_log | Có kết quả terminal: 22 ảnh cá nhân 0 ERROR/5 WARN; extra 0/0; peer 22 ERROR/5 WARN; diff 156 dòng |
| 5 | ChatGPT | Hỗ trợ soạn các báo cáo theo mẫu từ kết quả đã cung cấp | Tạo dự thảo CSV/MD và điền quyết định schema có căn cứ | Cần kiểm lại thông tin còn thiếu, quyết định dựa trên ảnh và bài cá nhân trước khi nộp |

**Nguồn đối chiếu peer:** Repo của Nguyễn Trọng Hùng (`2A202603147`): https://github.com/ngtronghung004-droid/K5-L2-DAY12-NguyenTrongHung-2A202603147-DMSOMS/tree/main. Hai file export peer do người dùng cung cấp, chưa xác minh nội dung trực tiếp trên GitHub.

**Vai trò của AI:** Hỗ trợ giải thích guideline, hướng dẫn lệnh và biên soạn bản thảo; các quyết định QC dựa trên ảnh thực tế và việc xác nhận với reviewer không được AI tự xác nhận.

**Cập nhật quyết định:** ChatGPT hỗ trợ lập phương án xử lý 5 WARN (D01, D05, D06, S01_ir) và chuẩn hóa 5 quyết định `frame_meta.modality` theo quy tắc. Các phương án WARN chưa được kiểm chứng trực tiếp bằng ảnh; chưa ghi là đã sửa trên CVAT hoặc đã được reviewer xác nhận.
