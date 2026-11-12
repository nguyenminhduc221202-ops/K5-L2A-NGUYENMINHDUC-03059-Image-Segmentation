# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp:03059
- Ngày / CVAT local: 11/11/2026 / CVAT local v2.x
- Công cụ đã dùng: CVAT local, sam3

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **82** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- **Ảnh, vị trí và object Medium đầu tiên tự vẽ:** Ảnh `000000181542.jpg`, đối tượng người đi bộ (class `person`) đang băng qua đường ở khu vực vạch kẻ đường bên trái ảnh.
- **Class và quy tắc tôi dùng để chọn biên:** Class `person`. Dùng công cụ Polygon vẽ thủ công ôm sát biên cơ thể, quần áo của người đi bộ; dừng biên chính xác tại điểm tiếp xúc giữa đế giày và mặt đường, tuyệt đối không vẽ lấn ra phần bóng đổ của người trên mặt đường.
- **Nếu dùng gợi ý sau đó:** Khi dùng thử mô hình SAM gợi ý cho chiếc xe máy (`motorcycle`) gần đó, AI tự động bắt cả vệt bóng đổ sẫm màu dưới gầm xe vào mask của xe; tôi đã dùng điểm trừ (`-Point`) trong AnyLabeling và chỉnh lại các nút polygon để loại bỏ phần bóng, giữ mask ôm sát lốp xe.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

**Task/ảnh/vùng:** Task `hard_panoptic`, ảnh `000000350023.jpg`, khu vực rặng cây xanh và thảm thực vật bên phải tuyến đường.
- **Lỗi thuộc loại:** Sai lớp / thiếu vật và bỏ sót vùng phủ (`vegetation`).
- **Bằng chứng tôi nhìn thấy:** Khi chạy tự động phát hiện, mô hình không nhận diện được các rặng cây xanh do nhãn `vegetation` mang tính chất vùng nền (stuff), dẫn đến việc vùng cây bị bỏ trống và AI lẹm sang lớp bầu trời (`sky`) hoặc tòa nhà (`building`).
- **Quy tắc và hành động sửa:** Chuyển sang dùng mô hình Segment Anything (SAM) click điểm trực tiếp (phím Q) lên các khối tán lá, kết hợp công cụ Brush trong CVAT để tô kín các tán cây và kẽ lá; sau đó gán nhãn chính xác là `vegetation` theo đúng `classes.json`.
- **Sau sửa đã Save và export lại chưa?** Đã Save trên CVAT và export lại file `hard_panoptic.zip`.
- **Kết quả kiểm tra cấu trúc:** Đã chạy script `python scripts/inspect_submissions.py --dir submissions` và đạt trạng thái `[OK]` cho cả 3 file zip (easy, medium, hard).

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. Ảnh `000000350023.jpg` (Hard) - Ranh giới mặt đường và vỉa hè | Gộp chung dải mép đường vào `road` do màu sắc nhựa đường và vỉa hè khá tương đồng; hoặc tách riêng `sidewalk`. | Dựa theo gờ bó vỉa (curb) và chức năng phân làn người đi bộ theo quy tắc của bài lab. | Quyết định tách riêng thành 2 vùng độc lập: `road` cho lòng đường xe chạy và `sidewalk` cho phần vỉa hè bên trên bó vỉa. |
| 2. Ảnh `000000350023.jpg` (Hard) - Cần cẩu xây dựng phía sau tòa nhà | Phân đoạn cần cẩu thành đối tượng riêng hay gộp vào tòa nhà / để nền trời? | Bảng nhãn của task không có class riêng cho máy móc công trình/cần cẩu (`crane`), chỉ có `building` và `sky`. | Quyết định gộp phần thân cần cẩu gắn liền với khối công trình vào `building`, các thanh giằng quá mảnh trên nền trời để thuộc về `sky` để tránh nhiễu biên. |
| 3. Ảnh `000000181542.jpg` (Medium) - Xe máy bị che khuất một phần | Tách chiếc xe máy bị che thành 2 instance khác nhau; hoặc giữ chung 1 instance ID. | Quy tắc Instance Segmentation: Các phần nhìn thấy của cùng một vật thể vẫn thuộc về một instance duy nhất (không vẽ phần bị che nhưng cùng 1 ID). | Quyết định gộp các phần nhìn thấy của chiếc xe vào chung 1 mask instance mang class `motorcycle`. |
