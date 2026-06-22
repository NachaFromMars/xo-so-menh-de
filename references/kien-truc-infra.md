# Kiến Trúc & Kill Switch & Bot & Routes

## Stack Kỹ Thuật

| Layer | Tech | Chi tiết |
|-------|------|---------|
| Mobile Shell | WebViewGold | Android wrapper |
| Frontend | Vue.js SPA | app.js 1.3MB + vendors.js 2.5MB + Framework7 UI |
| Backend API | Laravel | `api.ktso.net/api/` (RESTful) |
| Realtime | Laravel Echo + Pusher | Broadcasting, live updates |
| CDN | Cloudflare | Server Singapore |
| Package | `com.kts.monitor` v1.0.5 | target SDK 35, min SDK 23 |

## Kill Switch — Qua Mắt Apple/Google

```
review_status === 1 → USER THẬT → hiện LÔ ĐỀ
review_status === 0 → REVIEWER  → hiện MINING (giả)

feature_lock = true → khóa cược khi hết Credit
```

**Màn hình giả cho reviewer:**
- `/miner/add-wallet` — Thêm wallet đào coin
- `/miner/view-wallet` — Xem wallet (2miners API)
- `/miner/edit-wallet` — Sửa wallet
- `/review/random-number` — Máy tạo số ngẫu nhiên
- `/review/calculator` — Máy tính

## Bot Telegram Tự Động

| Config | Bot | Chức năng |
|--------|-----|----------|
| `allow_active_bot_mn` | Bot Miền Nam | Parse → đặt cược MN |
| `allow_active_bot_mt` | Bot Miền Trung | Parse → đặt cược MT |
| `allow_active_bot_mb` | Bot Miền Bắc | Parse → đặt cược MB |

**Workflow:** User gửi tin Telegram → Bot nhận → Parser Engine → report_json → đặt cược → `bot_auto_send_summary_report` cuối ngày

**Routes Bot:**
- `/bot/tao-moi` — Tạo bot
- `/bot/tao-moi-ke-tin` — Bot kế thừa tin
- `/bot/tao-moi-nhan-tin` — Bot nhận tin
- `/bot/view` — Xem bot
- `/bot/config` — Cấu hình
- `/bot/overpoint` — Giới hạn cược

## Bản Đồ Routes

**Auth:** `/dang-nhap`, `/dang-ky`, `/quen-mat-khau`, `/doi-mat-khau`, `/doi-mat-khau-cap-2`

**Gambling:** `/xu-ly/:region/:playStyle`, `/xu-ly/.../khach-hang/:id`, `/xu-ly/.../them-tin-nhan`, `/xu-ly/.../sua-tin-nhan/:id`, `/du-chuan/:region/:playStyle`

**Config:** `/cu-phap`, `/cu-phap/tao-moi`, `/cu-phap/chinh-sua`, `/cai-dat-hien-thi`, `/cai-dat-cach-choi`, `/cai-dat-luu-tru`

**CRM/Agent:** `/khach-hang`, `/khach-hang/tao-moi`, `/khach-hang/chi-tiet/:id`, `/quan-ly-dai-ly`, `/tao-tai-khoan-khach`, `/tao-ma-gioi-thieu`

**Report:** `/bao-cao-ngay`, `/bao-cao-ngay-v2`, `/bao-cao-ngay-v3`

**Finance:** `/hoa-hong-gioi-thieu`, `/nap-rut-usdt`, `/mua-credit`, `/mua-usdt`

**Other:** `/bang-nhiem-vu`, `/ca-nhan`, `/test-tin`, `/bao-tri`, `/inbox/*`

## Infra & Third-Party

| Service | Mục đích |
|---------|---------|
| Cloudflare | CDN + DDoS protection |
| Pushwoosh | Push notifications (chính) |
| OneSignal | Push notifications (backup) |
| RevenueCat | In-app subscriptions |
| Rollbar | Error tracking |
| Google AdMob | Quảng cáo (màn mining giả) |
| Facebook Audience Network | Quảng cáo |
| Firebase Analytics | User tracking |
| Laravel Echo + Pusher | Realtime updates |
| Google Billing | In-app purchase |
