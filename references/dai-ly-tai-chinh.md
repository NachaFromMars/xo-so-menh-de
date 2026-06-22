# Hệ Thống Đại Lý (Agent System)

## Phân Loại Khách

| Type | Vai trò | Đặc điểm |
|------|---------|----------|
| `type = 1` | Khách hàng (Customer) | Đặt cược, xem kết quả. sum_total giữ nguyên dấu |
| `type = 4` | Đại lý (Agent) | Quản lý khách, config deal riêng. sum_total ĐẢO DẤU |

## config_deal — 10 Bộ Rate Cho Mỗi Đại Lý

```json
customer.config_deal = {
  "mn":        { "total_percent": 100, "line_percent": 0, ...rates },
  "mn_mtay":   { "total_percent": 100, "line_percent": 0, ...rates },
  "mn_buonme": { "total_percent": 100, "line_percent": 0, ...rates },
  "mt":        { "total_percent": 100, "line_percent": 0, ...rates },
  "mt_mtay":   { "total_percent": 100, "line_percent": 0, ...rates },
  "mt_buonme": { "total_percent": 100, "line_percent": 0, ...rates },
  "mb":        { "total_percent": 100, "line_percent": 0, ...rates },
  "mb_hanoi":  { "total_percent": 100, "line_percent": 0, ...rates },
  "mb_ngan":   { "total_percent": 100, "line_percent": 0, ...rates },
  "mb_buonme": { "total_percent": 100, "line_percent": 0, ...rates }
}
```

## Hệ Thống Ăn Chia

- `total_percent` — % ăn chia (default 100%). VD: 80% = đại lý giữ 80%, gốc giữ 20%
- `line_percent` — % hồi theo dòng (chiết khấu thêm cho đại lý)

### Công thức final

```
final = sum_total × total_percent%
sum_final = final - (final × line_percent%)   // nếu line_percent > 0
```

## Quản Lý Cây Đại Lý

- Quan hệ `parent → childs` (đại lý cha → đại lý con)
- Mỗi đại lý có config_deal riêng cho MỖI VÙNG (10 bộ rate)
- Routes:
  - `/quan-ly-dai-ly` — Quản lý đại lý
  - `/tao-tai-khoan-khach` — Tạo TK khách
  - `/tao-ma-gioi-thieu` — Tạo mã giới thiệu

## Tài Chính (USDT + Credit)

| Thành phần | Chi tiết |
|-----------|---------|
| USDT Wallet | BSC-20 + TRC-20 |
| Credit System | Mua credit bằng USDT → dùng credit chơi |
| Tỷ giá | `usdt_buy` (mua) / `usdt_sell` (bán) |
| Balance VND | `wallet_balance × usdt_buy = VND` |
| Hoa hồng | `spent_usdt`, `commission_usdt` |
| Subscription | RevenueCat (tính năng premium) |

### Feature Lock

```
system_credit ≤ 0  → feature_lock = true → khóa tất cả cược
system_credit ≤ 90 → cảnh báo "Bạn đã gần hết Credit"
Buôn Mê editions bypass cảnh báo
```
