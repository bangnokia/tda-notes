---
repository: https://github.com/swebvn/lucommerce
---
#laravel
#livewire
#alpinejs

Project dùng cho việc provision nhanh chóng các store bán merch theo chủ đề. (Sản phẩm chủ yếu lấy từ Redbubble).

Domain chính: `tdalunar.com`

## Mô hình hệ thống

Từng server sẽ được setup 1 website chính, gọi là hub (central). Các hub sẽ tạo các website trên server. Quy tắc đặt tên `s1.tdalunar.com`, `s2.tdalunar.com`, ...

## Khởi tạo 1 store

Truy cập vào từng hub để khởi tạo store. Có 1 việc chính có thể làm song song với nhau

### Trỏ domain về hub

Có 2 cách đề trỏ domain về hub, ví dụ dưới đây hướng dẫn khi sử dụng Cloudflare.
1. Tạo CNAME record cho root domain, trỏ về `sxx.tdalunar.com` (domain của hub).
2. Tạo A record trỏ về IP của từng hub (không khuyến khích).
*Chú ý không được sử dụng proxied mode (đám mây vàng) của Cloudflare*

### Provision website trên hub

- domain: domain chính của website
- keyword: keyword chính cho việc SEO, đồng thời cũng là chủ đề của website)
- related keywords: các keyword liên quan đến keyword chính (hiện tại sử dụng cho việc lọc title sản phẩm khi crawl về)
- topic: chủ đề web, ví dụ: anime, movie, tv series,...




