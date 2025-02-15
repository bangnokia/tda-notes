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

### Lunar hub là gì?

Lunar hub các các website chính cài trên từng server, phụ trách cho việc khởi tạo các website trên server này.

Admin url: `hubdomain/hub`

Giao diện admin của hub có thể thực hiện các hành động sau:
- Toggle chế độ maintenance của website
- Download, upload (replace) database của 1 store (hỗ trợ cho việc việc chuyển store từ server này sang server khác)
- Quản lý store

### Tenant store là gì?

Tenant hay là các store bán sản phẩm, được tạo trên 1 hub.

Admin url: `domain/lunar

Admin của store sử dụng cho việc quản lý sản phẩm,...

### Payment gateway

Các store lunar ko sử dụng checkout trên web, mà chúng ta sẽ thông qua 1 payment gateway khác (Woocommerce), hay còn gọi là redirect pay. Giảm thiểu việc cài đặt các tài khoản trên hàng ngàn store, khó trong việc quản lý.

## Stack sử dụng

Ngôn ngữ:
- Laravel
- AlpineJS
- Livewire
- SQLite

Server:
- Caddy: cấu hình đơn giản, auto TLS thay vì Nginx và Certbot.
- SQLite
- PHP-FPM

Storage:
- Bunny

Kiến trúc website sử dụng 2 package chính là
- [Lunarphp](https://lunarphp.com) cho các function và backend liên quan đến ecommerce
- [TenancyForLaravel](https://tenancyforlaravel.com/) dùng cho kiến trúc multi tenancy, 1 server sẽ phục vụ nhiều website.

Các feature có thể viết như 1 app Laravel cơ bản, hoặc chia thành các package trong thư mục `packages` của dự án. Điều này giúp cho việc maintain code nhẹ nhàng hơn.

## Khởi tạo 1 hub

Server requirements:
- Ubuntu 22.04

Các bước khỏi tạo hub sẽ thực hiện trên Jenkins

1. Add deploy key dùng job: http://jenkins.sweb.vn/job/sweb/job/Add_user_deploy/
2. Provision server cho hub dùng job: http://jenkins.sweb.vn/job/lunar2/job/provision/

## Khởi tạo 1 store

Truy cập vào từng hub để khởi tạo store. Có 2 bước chính là trỏ domain và khởi tạo, có thể làm song song, không ảnh hưởng đến nhau.

### Trỏ domain về hub

Có 2 cách đề trỏ domain về hub, ví dụ dưới đây hướng dẫn khi sử dụng Cloudflare.
3. Tạo CNAME record cho root domain, trỏ về vd `s20.tdalunar.com` (domain của hub).
4. Tạo A record trỏ về IP của từng hub (không khuyến khích).
*Chú ý không được sử dụng proxied mode (đám mây vàng) của Cloudflare*

### Provision website trên hub

- domain: domain chính của website
- keyword: keyword chính cho việc SEO, đồng thời cũng là chủ đề của website)
- related keywords: các keyword liên quan đến keyword chính (hiện tại sử dụng cho việc lọc title sản phẩm khi crawl về)
- topic: chủ đề web, ví dụ: anime, movie, tv series,...

## Bulk khởi tạo store

Khởi tạo thông qua [N8N form](http://n8n.customedge.co/form/12afc66b-68b4-4fba-ae8e-8e35869d73e8)


Chuẩn bị các thông tin qua file csv, chọn server và submit. Chú ý trỏ domain về đúng server của hub.

Định dạng file csv có thể xem trên file mẫu ở form n8n, hoặc format cơ bản như sau

| domain           | keyword     | similar_keywords        | topic     |
| ---------------- | ----------- | ----------------------- | --------- |
| chillguy.store   | chill guy   | chill guy, chilling guy | meme      |
| thesimpson.store | the simpson | the simpson             | tv series |

## Các action khác

### Thay đổi pass user hàng loạt cho toàn bộ stores

Việc này có thể thực hiện trên [[Jenkins]] thông qua [artisan-command](http://jenkins.sweb.vn/job/lunar2/job/run-artisan-command/) runner.

Command: 
```bash
php artisan tenants:change-password "admin@sweb.vn" "newpassword"
```
