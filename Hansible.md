---
repository: github.com/swebvn/hansible
---
## Giới thiệu

Repo chứa các #Ansible playbook được tổng hợp lại từ trước, quản lý tập trung thay vì mỗi playbook viết ở 1 repository khác nhau.

Hiện tại chỉ có các playbook hay dùng, 1 số legacy playbook sẽ cần phải check trên [[Jenkins]] để biết thêm thông tin. Jenkins cũng là máy control node, 1 số playbook cho các site Wordpress có thể cần phải chạy trên server này (yêu cầu python2, python3 ko support).

Hiện tại có các playbook support cho provision server Lunar2.

Các utilities command có thể viết trong đây, máy control có thể run trên toàn bộ các nodes.

## Cấu trúc

Project custom lại roles_path ko giống default ansible, chú ý cấu hình trong `ansible.cfg`

-- inventories:  danh sách các server cho web lunar
-- playbooks: danh sách các playbook
-- roles: danh sách các roles

## Bảo mật

Hiện tại project đang ở trạng thái public 😂, có 1 số vấn đề chưa giải quyết đề chuyến sang private.