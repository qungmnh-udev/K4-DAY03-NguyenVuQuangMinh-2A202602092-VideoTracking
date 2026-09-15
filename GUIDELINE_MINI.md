# Mini annotation guideline — Ngày 3 (tracking)

> Điền file này **trong lúc gán nhãn**, không phải sau khi xong. Mỗi lần bạn dừng
> lại nghĩ "cái này tính sao nhỉ?" thì đó là một dòng phải ghi vào đây.
>
> Đây là tài liệu mà người gán nhãn tiếp theo sẽ đọc để làm giống bạn. Nếu hai
> người trong nhóm gán khác nhau, gần như luôn là vì file này chưa nói rõ — chứ
> không phải vì ai kém.

Nhóm / tên: `Nguyễn Vũ Quang Minh`
Clip: `clip_01`, `clip_02`

---

## 1. Phạm vi: gán cái gì, không gán cái gì

Một lớp duy nhất: **`vehicle`** — xe bốn bánh (xe con, van, xe buýt, xe tải).

| Gán | Không gán |
| --- | --- |
| xe con, SUV, taxi, xe bán tải | người đi bộ |
| van, minivan | xe đạp |
| xe buýt, minibus | **xe máy / mô tô** |
| xe tải, xe đầu kéo | xe trong ảnh quảng cáo, trong gương, dưới bóng nước |

Bổ sung của nhóm (nếu có): `Không có`

## 2. Luật ID — phần quan trọng nhất

| Tình huống | Luật của nhóm | Vì sao |
| --- | --- | --- |
| Xe bị che một phần rồi hiện lại | giữ nguyên ID nếu bị che **dưới 50 frame** (mặc định của lab: 25 frame = 2 giây @ 12.5 fps) | `2s có thể là khoảng thời gian khá thấp để có thể xác định được vật thể gốc đã rời khung hình hay chưa. ` |
| Xe bị che lâu hơn ngưỡng trên | `Đánh giá dựa trên các frame khác nhau để xác định có nên giữ nguyên ID hay không.` | `...` |
| Xe rời khung hình rồi quay lại | mặc định: **track mới** | `Có thể giữ track cũ nếu như đủ dữ kiện để xác định ` |
| Hai xe cắt nhau / chồng lên nhau | `giữ nguyên id 2 xe cắt nhau/chồng nhau, đặt occluded properties` | `Vì trừ khi 2 xe hoàn toàn chồng lên nhau thì vẫn có thể xác định được vật thể` |

## 3. Luật bbox

| Tình huống | Luật của nhóm |
| --- | --- |
| Xe bị cắt bởi rìa ảnh | bbox chạm đúng rìa, không đoán phần ngoài ảnh |
| Xe bị xe khác che một phần | bbox ôm phần **nhìn thấy được** |
| Xe vừa xuất hiện, còn rất nhỏ / rất mờ | bắt đầu track từ frame đầu tiên xác định được là xe bốn bánh; ngưỡng nhóm chọn: `bắt đầu track từ frame đầu tiên xác định được đặc điểm xe` |
| Xe đang đỗ, không di chuyển | `giữ track` |
| Keyframe đặt dày ở đâu | `Keyframe đặt dày ở các frame có nhiều xe bị occlude lên nhau, ra vào khung hình.` |

## 4. Ít nhất ba ca mơ hồ đã gặp thật

Ghi **frame cụ thể** và **ID cụ thể**, không ghi chung chung.

### Ca 1
- Clip / frame / ID: `clip_01`,`VEHICLE 5`,`frame 000060.jpg`
- Tình huống: `Xe con (VEHICLE 5) đi song song cùng vận tốc và hướng với xe buýt VEHICLE 4 nhưng bị occluded. (Đầu xe)`
- Quyết định: `Không track tại frame này`
- Lý do: `Thiếu nhiều dữ kiện cho việc quyết định, có thể làm ảnh hưởng tới chất lượng dữ liệu`

### Ca 2
- Clip / frame / ID:`clip_01`, `VEHICLE 6`, `000078.jpg`
- Tình huống: `Xe con đi song song cùng chiều và vận tốc với xe buýt VEHICLE 4 nhưng bị occluded (Đuôi xe)`
- Quyết định: `Không track tại frame này`
- Lý do: `Thiếu nhiều dữ kiện, không rõ ràng nên không track.`

### Ca 3
- Clip / frame / ID: `clip_01`, `VEHICLE 2`, `000190.jpg`
- Tình huống: `Xe con (VEHICLE 2) đang đỗ trên đường, đã track nhưng bị xe khác đi qua che mất`
- Quyết định: `Giữ track, đổi occluded properties thành Occluded`
- Lý do: `Xe vẫn đủ dữ kiện để được đánh giá và track`

## 5. Sửa gì sau khi chấm với gold và sau khi kiểm chéo

Luật nào trong file này hoá ra còn thiếu hoặc còn mơ hồ? Viết lại cho rõ:

- `Xe đang đỗ, không di chuyển trong 80 frames thì drop track, tránh làm rác dữ liệu`
- `...`
