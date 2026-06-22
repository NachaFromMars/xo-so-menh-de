# Hệ Thống Cú Pháp & Parser Engine

## Cấu Trúc Cú Pháp Đặt Cược

```
[tên đài] [số cược] [số tiền][đơn vị] [loại cược]
```

**Ví dụ:**
- `tp 36 50n de` → TP HCM, số 36, 50 nghìn, đề đuôi
- `dn 123 10n bac` → Đà Nẵng, số 123, 10 nghìn, ba càng
- `mb 45.67 20n dx` → Miền Bắc, đá xiên 45-67, 20 nghìn

Parser chạy phía server (Laravel), frontend gửi raw text.

## Bảng Aliases Cú Pháp

| Lệnh | Aliases | Loại cược |
|-------|---------|-----------|
| Bao lô | `bao, baolo, blo, b, lo, bl` | 2C Bao |
| Bao lô đảo | `baodao, daobao, bldao` | 2C Bao đảo |
| Đầu đuôi | `dauduoi, daudui, dd` | 2C ĐĐ |
| Đầu | `dau` | 2C Đầu riêng |
| Đuôi / Đề | `duoi, dui, de, dacbiet` | 2C Đuôi / Đề |
| Đá thẳng | `dathang, thang, dat` | 2C Đá T |
| Đá xiên | `daxien, dx, xien, xi` | 2C Đá X |
| Xiên 2 | `xienhai, xi2` | 2C ĐáX2 |
| Xiên 3 | `xienba, xi3` | 2C ĐáX3 |
| Xiên 4 | `xienbon, xi4` | 2C ĐáX4 |
| Xỉu chủ | `xc, xchu, xiuchu, xiu, tieulo, tlo` | 3C XC |
| Bảy lô | `baylo, bay` | 2C B7 |
| Tám lô | `tamlo, btam` | 2C B8 |
| Ba càng | `bacang, bac, cang` | 3C Bao |
| Ngang (5 số) | `ngang, namso` | 1C |
| Giải nhất | `giainhat, gnhat` | GNhat |
| Tài xỉu / Kéo | `taixiu, keotai, keoxiu, chan, le, cao, thap, tongchan, tongle` | Kéo |

## Đơn Vị Tiền

| Ký hiệu | Giá trị |
|----------|---------|
| `n` hoặc `k` | ×1.000 (nghìn) |
| `tr` | ×1.000.000 (triệu) |
| Không ghi | Đơn vị mặc định (nghìn) |

## Bảng Mã Loại Cược (command_play_style)

| Code | Tên đầy đủ | Phân loại |
|------|-----------|-----------|
| `1C` | Một con (Ngang/5 số) | 1 con |
| `GNhat` | Giải nhất | Đặc biệt |
| `2CB` | 2 con bao lô | 2 con |
| `2CĐ` | 2 con đầu đuôi | 2 con |
| `2CB7` | 2 con bảy lô | 2 con |
| `2CB8` | 2 con tám lô | 2 con |
| `ĐáT` | Đá thẳng | Đá |
| `ĐáX` | Đá xiên (1 đài) | Đá |
| `ĐáX2` | Đá xiên 2 đài | Đá |
| `ĐáX3` | Đá xiên 3 đài | Đá |
| `ĐáX4` | Đá xiên 4 đài | Đá |
| `3CB` | 3 con bao | 3 con |
| `3CB7` | 3 con bảy | 3 con |
| `3CXC` | 3 con xỉu chủ | 3 con |
| `4C` | 4 con số | 4 con |
| `An Ủi` | Ăn an ủi (2C) | Bonus |
