# Dựng mô hình Home-Lab

## 1. Thông tin chung về card mạng phần mềm ảo hóa: Có các chế độ sau:
- VM0 – Bridge
- VM8 – NAT
- VM1-7 và VM9-19: Host Only
- Lan segment
Trong mô hình Home-Lab này sử dụng VM8 (NAT), (các bạn có thể sử dụng VM0 cũng được). 
Tuy nhiên lưu ý là tất cả lớp đều bật chế độ tự động cấp DHCP. Để tắt dịch vụ DHCP của VM ta làm như sau: 
Trên thẻ tab của vmware vào phần Edit→Vitrual Network Editor → Change Setting và bỏ dấu tích phần DHCP setting và bỏ tích trong phần Use local DHCP to distribute IP to VMwaresD
<img src=https://i.imgur.com/inMO3V9.png>

## 2. Thông tin máy vật lý:
- Tạo tên máy ảo theo thông tin sau:
Tên máy vật lý Dịch vụ Hostname
Debian-01 DNS-DHCP debian-01.cd41qtm.net
Debian-02 FTP-WEB Debian-02.cd41qtm.net
Debian-03 NFS-SAMBA Debian-03.cd41qtm.net
Deian-client Client-01.cd41qtm.net
Win7 Client-02.cd41qtm.net
- Gán địa chỉ IP như sau:
Tên máy IP GW DNS
Debian-01 192.168.x.110 192.168.78.2 8.8.8.8
Debian-02 192.168.x.111 192.168.78.2 8.8.8.8
Debian-03 192.168.x.112 192.168.78.2 8.8.8.8
Request dhcp
Requset dchp
### 2.1. Cấu hình hostname:
Cách 1:
#hostnamectl set-hostname debian-01.cd41qtm.net
Cách 2:
| #vi /etc/hostname (nhập nội dung vào file hostname)
ấn nút i (chế độ insert cho phép nhập nội dung)
debian-01.cd41qtm.net
ấn esc chuyển sang chế độ command
Nhập :wq! (lưu và thoát)
Cách 3:
#echo “debian-01.cd41qtm.net” > /etc/hostname
### 2.2. Cấu hình địa chỉ IP:
Bước 1:
| #nano /etc/network/interfaces (nhập nội dung sau)
auto eth0
iface eth0 inet static
address 192.168.78.110
netmask 255.255.255.0
network 192.168.78.0
broadcast 192.168.78.255
gateway 192.168.78.2
ấn Esc và gõ :wq! (lưu và thoát)
Bước 2: Khởi động lại dịch vụ network
| #service restart networking.service
Hoặc
| #systemctl restart networking
Hoặc
| #/etc/init.d/networking restart
# 3. Tài khoản truy cập:
Tài khoản Linux Windows
Username root/skill41 Skill41
Password qazXSW123 qazXSW123
Nếu đã gắn thì chuyển sang bước 2.
Bước 2: Cài đặt phần mềm ssh
#apt-get install openssh-server
Bước 3: Cấp quyền truy cập cho root (tạm thời mở quyền root truy cập)
#vim /etc/ssh/sshd_config
ấn shift : gõ set nu (sẽ hiện thị số dòng )
Tìm đến dòng thứ 28:
PermitRootLogin without-no
Sửa thành
PermitRootLogin yes
ấn Esc và gõ :wq!
# 4. Cấu hình ssh cho tất cả các máy Debian:
B1: cài đặt ssh
Trước khi cài đặt kiểm tra lại trong phần setting xem ổ đĩa đã gắn fille iso vào chưa.
Nếu chưa thì tìm đường dẫn chính xác đến file chứa tập tin iso, sau đó tích vào nút
Connected và Connect at power on (Hình bên dưới)
Nếu đã gắn thì chuyển sang bước 2.
Bước 2: Cài đặt phần mềm ssh
|#apt-get install openssh-server
Bước 3: Cấp quyền truy cập cho root (tạm thời mở quyền root truy cập)
|#vim /etc/ssh/sshd_config
ấn shift : gõ set nu (sẽ hiện thị số dòng )
Tìm đến dòng thứ 28:
PermitRootLogin without-no
Sửa thành
PermitRootLogin yes
ấn Esc và gõ :wq!
Nếu đã gắn thì chuyển sang bước 2.
Bước 2: Cài đặt phần mềm ssh
#apt-get install openssh-server
Bước 3: Cấp quyền truy cập cho root (tạm thời mở quyền root truy cập)
#vim /etc/ssh/sshd_config
ấn shift : gõ set nu (sẽ hiện thị số dòng )
Tìm đến dòng thứ 28:
PermitRootLogin without-no
Sửa thành
PermitRootLogin yes
ấn Esc và gõ :wq!

Bước 4: Khởi động lại dịch vụ ssh
#service restart ssh
Hoặc
#systemctl restart ssh
Hoặc
#/etc/init.d/ssh restart
5. Tạo Banner khi truy cập qua ssh
Bước 1: Sửa file /etc/ssh/sshd_config
#vim /etc/ssh/sshd_config
Tìm đến Dòng 72 ấn i và Bỏ dấu # (dòng 72 #Banner /etc/issue.net)
thành
72 Banner /etc/issue.net
Ấn Esc và nhập :wq! (lưu và thoát)
Bước 2: Chỉnh sửa file /etc/issue.net như sau:
ấn i để chỉnh sửa nội dung:
Ấn Esc và nhập :wq! (lưu và thoát)
Bước 3: Khởi động lại dịch vụ ssh
#service restart ssh
Hoặc
#systemctl restart ssh
Hoặc
#/etc/init.d/ssh restart
Làm lại với tất cả các máy linux (Debian)
Riêng máy win7 thì đặt tên theo client02.cd41qtm.net và để chế độ dhcp
