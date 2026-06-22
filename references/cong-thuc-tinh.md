# Công Thức Tính Số Đề & Tổng Kết

## report_json — Cấu trúc 4 tầng

Mỗi tin nhắn cược sau khi parse tạo `report_json` gồm 4 tầng: `1c`, `2c`, `3c`, `4c`.

Mỗi tầng chứa 6 trường:

| Field | Ý nghĩa |
|-------|---------|
| `point` | Số điểm (đơn vị cược, chưa nhân tiền) |
| `cost` | Tiền đặt SAU chiết khấu (`point × rate`) |
| `cost_without_rate` | Tiền đặt GỐC (chưa chiết khấu) |
| `win` | Tiền ăn SAU tỷ lệ ăn (`point × win_rate × trúng`) |
| `win_without_win_rate` | Tiền ăn GỐC (chưa nhân tỷ lệ ăn) |
| `result` | Kết quả cuối (`cost - win`) |

### Công thức liên hệ

```
cost = cost_without_rate × rate
win = win_without_win_rate × win_rate
result = cost - win   (dương = nhà cái thu, âm = nhà cái bù)
```

### Sub-fields theo tầng

**1c:** `ngang`

**2c:** `baolo, baylo, tamlo, dau, duoi, ke, dat, dx, dx_2, dx_3, dx_4, giainhat, daunhat, ui`

**3c:** `baolo, baylo, dat, dx, dau, duoi, ke`

**4c:** `dat, dx, baolo, dau, duoi, baylo`

---

## Tỷ Lệ Ăn / Tỷ Lệ Cá (Rate Defaults)

### MN & MT

| Loại | Rate (chiết khấu) | Win Rate (ăn) |
|------|-------------------|---------------|
| 2C Bao lô | 0.75 | ×75 |
| 2C Đầu đuôi | 0.75 | ×75 |
| 2C Bảy lô | 0.75 | ×75 |
| 2C Tám lô | 0.75 | ×75 |
| 2C Đá thẳng | 0.75 | ×700 |
| 2C Đá xiên | 0.75 | ×550 |
| 3C Bao | 0.65 | ×650 |
| 3C Đầu đuôi | 0.65 | ×650 |
| 4C Bao | 0.65 | ×5,500 |
| 4C Đầu đuôi | 0.65 | ×5,500 |

### MB (khác biệt)

| Loại | Rate | Win Rate | So với MN/MT |
|------|------|----------|-------------|
| 2C Bao/ĐĐ/B7/B8 | 0.75 | ×75 | Giống |
| 2C Đá thẳng | 0.75 | ×650 | ↓ từ ×700 |
| 2C Đá xiên 1 đài | 0.75 | ×650 | ↑ từ ×550 |
| 2C Đá xiên 2 đài | 56 | ×1,000 | Riêng MB |
| 2C Đá xiên 3 đài | 52 | ×4,000 | Riêng MB |
| 2C Đá xiên 4 đài | 45 | ×10,000 | Riêng MB |
| 3C / 4C | 0.65 | Giống MN | — |

### Rate Bonus Type

```
rate_bonus_type:    MN/MT = 2, MB = 3
rate_bonus_type_dx: MN/MT = 3, MB = 1
```

Rate mặc định có thể bị override bởi `config_deal` của từng đại lý/khách hàng.

---

## Công Thức Tổng Kết

### sum_total — Tổng lãi/lỗ (BigNumber precision)

```
sum_total = Σ(tất cả cost) - Σ(tất cả win)

CỘNG:
+ sum_cost_1c, sum_cost_giainhat
+ sum_cost_2c_bao, sum_cost_2c_bay, sum_cost_2c_tam, sum_cost_2c_dd
+ sum_cost_2c_dat, sum_cost_2c_dax, sum_cost_2c_dax2..4
+ sum_cost_3c_bao, sum_cost_3c_bay, sum_cost_3c_xc
+ sum_cost_4c

TRỪ:
- sum_win_1c, sum_win_giainhat
- sum_win_2c_bao, sum_win_2c_bay, sum_win_2c_tam, sum_win_2c_dd
- sum_win_2c_ui (an ủi)
- sum_win_2c_dat, sum_win_2c_dax, sum_win_2c_dax2..4
- sum_win_3c_bao, sum_win_3c_bay, sum_win_3c_xc
- sum_win_4c
```

### Đảo dấu theo loại khách

```
type === 1 (Khách hàng): return sum_total     (dương = nhà cái THU)
type !== 1 (Đại lý):     return -sum_total    (ĐẢO DẤU, nhìn từ góc khách)
```

### sum_result — Kết quả sau ăn chia

```
sum_result = Σ tất cả report_json[tầng].result[sub-field]

1c: ngang
2c: 13 sub-fields (giainhat, daunhat, dat, dx, dx_2..4, baolo, dau, duoi, baylo, tamlo, ui, ke)
3c: 7 sub-fields (dat, dx, baolo, dau, duoi, baylo, ke)
4c: 6 sub-fields (dat, dx, baolo, dau, duoi, baylo)
```

### Hiển thị UI

```
sum_total >= 0 → xanh lá (forestgreen) → "Thu: [sum_total] x [total_percent]% = [final]"
sum_total < 0  → đỏ (crimson) → "Bù: [sum_total] x [total_percent]% = [final]"

Nếu line_percent > 0:
  "- ([line_percent]% hồi) = [sum_final]"
```

---

## Giới Hạn Cược (Overpoint System)

| Field | Áp dụng cho |
|-------|------------|
| `over_point_dau` | Giới hạn điểm Đầu |
| `over_point_duoi` | Giới hạn điểm Đuôi / Đề |
| `over_point_bao` | Giới hạn điểm Bao lô |
| `over_point_tamlo` | Giới hạn Tám lô (chỉ MB) |
| `over_point_da` | Giới hạn Đá thẳng |
| `over_point_dx` | Giới hạn Đá xiên |

**Phạm vi:** Vùng → Ngày → Đài → Loại cược → Con số

**Chặn số kẹt:** Tự động block con số bị cược quá nhiều.

Route quản lý: `/bot/overpoint`
