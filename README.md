# xo-so-menh-de — Technical blueprint of the XSMD Vietnamese lottery system

> Complete reverse-engineered documentation of the Xổ Số Mệnh Đề platform — architecture, syntax parser, payout formula, agent system, and full infrastructure stack.

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blueviolet)](https://github.com/NachaFromMars)

## Overview
xo-so-menh-de is a complete technical blueprint of the XSMD Vietnamese online lottery platform, reverse-engineered from APK `com.kts.monitor` v1.0.5 and Web SPA `app.ktso.net`. Documents: architecture, kill switch, betting syntax parser, payout formulas, agent system, wallets, Telegram bot, and infra.

## Features
- **Architecture:** WebViewGold + Vue.js SPA + Laravel backend
- **Kill switch:** `review_status` 0=fake (Apple/Google review) / 1=real lottery
- **Parser:** 60+ stations, 3 regions (MN/MT/MB), 10 zones, natural Vietnamese syntax, 16+ bet types
- **Payout:** 4-tier formula, MN/MT/MB default odds, overpoint + number blocking
- **Agent system:** tree structure, 10 rate sets/agent
- **Finance:** USDT wallet (BSC-20 + TRC-20), Credit, Feature Lock
- **Infra:** Cloudflare, Pusher, Firebase, Rollbar, RevenueCat, AdMob, Telegram bot

## Trigger Keywords (OpenClaw)
xổ số, lô đề, mệnh đề, XSMD, cú pháp cược, parser engine, tỷ lệ ăn, đại lý, overpoint, kill switch, ktso

---
Part of the [NachaFromMars](https://github.com/NachaFromMars) OpenClaw skill ecosystem.
