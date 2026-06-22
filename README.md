---
name: xo-so-menh-de
description: "Blueprint kỹ thuật hoàn chỉnh hệ thống Xổ Số Mệnh Đề (XSMD) — reverse engineered từ APK com.kts.monitor v1.0.5 + Web SPA app.ktso.net. Bao gồm: kiến trúc (WebViewGold + Vue.js + Laravel), kill switch qua mắt Apple/Google (review_status), 60+ đài xổ 3 miền (MN/MT/MB) với 10 vùng cược, parser engine cú pháp tự nhiên tiếng Việt, 16+ loại cược (1C→4C + Đá + Kéo), công thức tính số đề (report_json 4 tầng), tỷ lệ ăn/cá default cho MN/MT/MB, công thức tổng kết (sum_total + đảo dấu), overpoint + chặn số kẹt, hệ thống đại lý (config_deal 10 bộ/agent), tài chính USDT + Credit + Feature Lock, bot Telegram 3 miền tự động, bản đồ 40+ routes, infra stack. Dùng khi: cần hiểu, phân tích, tái tạo, hoặc tham khảo hệ thống lô đề online kiểu Việt Nam. Triggers: xổ số, lô đề, mệnh đề, XSMD, cú pháp cược, parser engine, tỷ lệ ăn, đại lý, overpoint, kill switch, WebViewGold, ktso."
---

# Xổ Số Mệnh Đề — Blueprint Kỹ Thuật

Phân rã kỹ thuật hoàn chỉnh hệ thống XSMD, reverse engineered từ APK + Web SPA.

**Source:** APK `com.kts.monitor` v1.0.5 + Web SPA `app.ktso.net` (app.js 6845bfd7)
**Ngày phân tích:** 13-14/03/2026

## Tổng Quan Kiến Trúc

```
MOBILE APP (WebViewGold)
├── review_status = 0 → MINING (giả, cho Apple/Google reviewer)
├── review_status = 1 → LÔ ĐỀ (thật, cho user)
└── Vue.js SPA (Framework7 UI + BigNumber.js)
         ↕
BACKEND (api.ktso.net/api/ — Laravel)
├── Parser Engine (cú pháp tự nhiên tiếng Việt)
├── Rate Engine (tỷ lệ ăn/cá, config_deal per agent)
├── KQXS (kết quả xổ số)
├── Agent System (cây đại lý, 10 bộ rate/agent)
├── USDT Wallet (BSC-20 + TRC-20)
├── Bot Telegram (3 miền: MN/MT/MB)
└── Overpoint + Chặn Số Kẹt + Config Deal
         ↕
INFRA: Cloudflare · Pusher · Firebase · Rollbar · RevenueCat · AdMob
```

## 14 Module Chi Tiết

Mỗi module có reference file riêng — chỉ đọc khi cần:

### Nhóm 1: Cú Pháp & Loại Cược
→ Đọc [references/cu-phap-parser.md](references/cu-phap-parser.md)
- Cấu trúc cú pháp đặt cược: `[đài] [số] [tiền][đơn vị] [loại]`
- Bảng aliases (17 lệnh, 50+ aliases)
- 16 loại cược: 1C, 2CB, 2CĐ, 2CB7, 2CB8, ĐáT, ĐáX, ĐáX2-4, 3CB, 3CB7, 3CXC, 4C, An Ủi, GNhat, Kéo
- Đơn vị tiền: `n/k` (nghìn), `tr` (triệu)

### Nhóm 2: Đài & Vùng
→ Đọc [references/he-thong-dai.md](references/he-thong-dai.md)
- 60+ đài: 21 MN + 14 MT + Hà Nội (MB)
- 10 vùng cược: mn, mn_mtay, mn_buonme, mt, mt_mtay, mt_buonme, mb, mb_hanoi, mb_ngan, mb_buonme

### Nhóm 3: Công Thức Tính & Tỷ Lệ
→ Đọc [references/cong-thuc-tinh.md](references/cong-thuc-tinh.md)
- report_json 4 tầng (1c/2c/3c/4c), 6 trường mỗi tầng
- Tỷ lệ ăn MN/MT vs MB (khác biệt đáng kể ở Đá xiên)
- Công thức sum_total, sum_result, đảo dấu theo type
- Overpoint system (6 loại giới hạn)

### Nhóm 4: Đại Lý, Tài Chính, Infra
→ Đọc [references/dai-ly-tai-chinh.md](references/dai-ly-tai-chinh.md)
- Hệ thống đại lý (type 1 vs 4), config_deal 10 bộ
- Ăn chia: total_percent + line_percent
- USDT BSC-20/TRC-20 + Credit system + Feature Lock

### Nhóm 5: Kiến Trúc, Kill Switch, Bot, Routes
→ Đọc [references/kien-truc-infra.md](references/kien-truc-infra.md)
- WebViewGold + Vue.js + Laravel stack
- Kill switch (review_status)
- Bot Telegram 3 miền tự động
- 40+ routes hoàn chỉnh
- 11 third-party services

## Công Thức Nhanh (Quick Reference)

```
cost = cost_without_rate × rate
win  = win_without_win_rate × win_rate
result = cost - win

sum_total = Σ(cost) - Σ(win)
  type=1 (khách): giữ nguyên dấu
  type=4 (đại lý): ĐẢO DẤU

final     = sum_total × total_percent%
sum_final = final - (final × line_percent%)
```

## Lưu Ý Quan Trọng

- Tất cả công thức trích từ app.js decompiled, không suy đoán
- Rate mặc định có thể bị override bởi config_deal từng đại lý
- MB có tỷ lệ Đá xiên KHÁC MN/MT (650/1000/4000/10000 vs 700/550)
- BigNumber.js dùng cho precision (tránh floating point)
- Buôn Mê editions bypass cảnh báo Feature Lock
