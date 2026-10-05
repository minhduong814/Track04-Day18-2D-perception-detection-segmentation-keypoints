# Báo cáo Bonus 4C — Val gốc và val lật gương

## Kết quả

| Model | Pose mAP50–95 trên val gốc | Pose mAP50–95 trên val lật gương | Mức giảm |
|---|---:|---:|---:|
| `flip_idx` giải phẫu | 0.4573 | 0.4388 | 0.0185 (4.0%) |
| `flip_idx` đồng nhất | 0.4169 | 0.2977 | 0.1192 (28.6%) |

## Nhận xét

Model dùng `flip_idx` đúng theo giải phẫu giữ kết quả khá ổn định sau khi lật ảnh, trong khi model dùng `flip_idx` đồng nhất giảm gần 29%. Điều này cho thấy model thứ hai đã học sai quy ước trái–phải và phụ thuộc mạnh vào hướng quay của con hổ.

Metric đã che lỗi là **Pose mAP50–95 khi chỉ đánh giá trên tập val gốc**. Do cả tập train và val gốc đều chỉ có hổ quay phải, model dùng `flip_idx` sai vẫn có thể học một quy ước nhất quán theo vị trí trên màn hình và đạt mAP tương đối tốt. Vì vậy, chênh lệch giữa hai model trên val gốc chỉ là 0.0404 và chưa phản ánh đầy đủ mức độ nghiêm trọng của lỗi. Khi đánh giá trên val lật gương, hướng quay thay đổi nên lỗi hoán đổi keypoint trái–phải mới bộc lộ rõ.

## Đề xuất thiết kế tập validation

Tập validation nên có số lượng hổ quay trái và quay phải tương đối cân bằng. Có thể bổ sung ảnh lật gương, nhưng phải hoán đổi nhãn keypoint trái–phải đúng theo `flip_idx` giải phẫu. Ngoài mAP tổng hợp, nên báo cáo mAP riêng cho từng hướng quay và thêm chỉ số đếm tỷ lệ các cặp keypoint trái–phải bị dự đoán đảo nhãn. Cách đánh giá này sẽ phát hiện được lỗi mà một giá trị mAP duy nhất trên tập val thiên lệch có thể che khuất.
