# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603148
- Ngày / CVAT local: 11/11/2026 · CVAT local (`localhost:8080`)
- Công cụ đã dùng: SAM (AI Tools → Interactors trong CVAT), Brush/Eraser để sửa mép; object Medium đầu tiên vẽ tay bằng Polygon/Brush

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `submissions/easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `submissions/medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `submissions/hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `submissions/cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `submissions/cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `submissions/cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `submissions/cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `submissions/cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `submissions/cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Không có export lỗi. Cả 9 ZIP qua `scripts/inspect_submissions.py` (đúng format, tên ảnh, tên class).

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: ảnh `000000181542`, người phụ nữ mặc áo dài trắng đi giữa vạch qua đường, chính giữa ảnh.
- Class và quy tắc tôi dùng để chọn biên: `person`. Bám theo mép tóc, áo dài và ống quần nhìn thấy, gồm cả hai tay buông theo người. Không kéo mask ra phần nền vạch kẻ đường phía sau và không đoán phần bị che.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: sau object đầu, tôi dùng SAM cho các object còn lại. Với người lái xe máy, SAM dễ gộp người và xe thành một vùng; tôi tách thành hai object riêng (`person` và `motorcycle`), vì đề yêu cầu mỗi vật là một instance. Ở bài nộp, người lái và xe máy phía trước bên trái ảnh này là hai mask riêng.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `hard_panoptic`, ảnh `000000350023`, hàng cây và khối nhà bên trái đường.
- Lỗi thuộc loại: phủ vùng + chồng lấn stuff (mask `vegetation` và `building` đè lên nhau, hàng cây bên trái gần như chưa có nhãn).
- Bằng chứng tôi nhìn thấy: overlay mask lên ảnh cho thấy khoảng 8.000 px nằm trong hơn một mask ở khu cây bên trái; khoảng 20% ảnh chưa thuộc mask nào, chủ yếu là hàng cây và vỉa hè bên trái.
- Quy tắc và hành động sửa: panoptic yêu cầu mỗi pixel thuộc đúng một segment. Tôi xóa phần chồng, tô lại hàng cây bên trái thành `vegetation`. Sau sửa: phần chồng còn khoảng 1.000 px, vùng chưa gán giảm từ 19,7% xuống 12,9%.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại (`hard_panoptic.zip`, 23:11), trước khi nhận reference.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): tự chạy `scoring/score.py` sau khi nhận reference: `hard_panoptic` PQ 0,356 → 0,403; PQ của `vegetation` 0,000 → 0,571. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1 · `hard_panoptic` ảnh `000000460147`, mặt đường rộng bên trái (làn có xe sedan trắng) | (a) `road` vì có vạch kẻ làn và xe chạy; (b) `sidewalk` | Nhìn ảnh: vạch kẻ làn và xe đang chạy → chức năng là lòng đường. Reference được phát lại gán phần lớn vùng này là `sidewalk`. | Tôi giữ `road` theo quy tắc ranh chức năng. **Câu hỏi cho coach:** reference ghi `sidewalk` ở vùng này là chủ ý hay lỗi nhãn? |
| 2 · `cp1_holes`, xe caravan (nhà kéo) bên trái sau xe máy | (a) `car`/`truck` vì là phương tiện; (b) không thuộc class nào | Class list chỉ có person/bicycle/car/motorcycle/bus/truck; caravan là rơ-moóc không động cơ, không khớp định nghĩa class nào. | Để trống, không gán nhãn. |
| 3 · `cp6_coverage`, xe buýt và xe tải ở giữa ảnh | (a) gán vào class gần nhất (`car`) để tăng độ phủ; (b) để trống | Class list cp6 không có `bus`/`truck`; quy tắc: tên class phải đúng `classes.json` của task, không ép nhãn. | Để trống; độ phủ cp6 thấp một phần vì lý do này, không phải bỏ sót. |
