<h1>Lab Linux cơ bản</h1>


<h2>1/ Thay đổi cổng kết nối SSH từ 22 thành 2022</h2>

Kiểm tra port ssh: <table style="width:100%;height:10%"><tr><th>netstat -nltp | grep sshd</th></tr></table>

Mở file config: <table style="width:100%;height:10%"><tr><th>vi /etc/ssh/sshd_config</th></tr></table> (Thay đổi port cũ -> port mới)

Sử dụng Ubuntu dung lệnh: <table style="width:100%;height:10%"><tr><th>sudo ufw allow 1998/tcp</th></tr></table>

Restart lại ssh: <table style="width:100%;height:10%"><tr><th>systemctl restart sshd</th></tr></table>

Kiểm tra lại

<h2>2/ Hướng dẫn khởi tạo SSH key</h2>
Ở đây tôi sử dụng Putty để khởi tạo file private & file public key:
<p>Open pyttygen</p>
<p>Chọn Load để khởi tạo key SSH</p>
<p>Lưu public key</p>
<p>Lưu private key</p>
Vào Ubuntu kiểm tra lại file copy ssh key vào file bằng lệnh
<table style="width:100%;height:10%"><tr><th>vi /home/phatnt/.ssh/authorized_keys</th></tr></table>
<h2>3/ Giới hạn kết nối SSH chỉ cho phép IP Wan của Toà nhà Núi Thành (Truy cập Kerio trước)</h2>

Giới hạn kết nối IP truy cập SSH #

<table style="width:100%;height:10%"><tr><th>$ vi /etc/hosts.allow</th></tr></table>

Thêm IP tòa nhà: *sshd: 118.69.62.113*

<table style="width:100%;height:10%"><tr><th>$ vi /etc/hosts.deny</th></tr></table>

Chặn toàn bộ các IP khác ngoài IP tòa nhà: *sshd: ALL*


<h2>4/ Cấu hình firewall với UFW để giới hạn các cổng kết nối được phép</h2>


Bật Firewall

<table style="width:100%;height:10%"><tr><th>$ sudo ufw enable</th></tr></table>

Cho phép cổng kết nối

<table style="width:100%;height:10%"><tr><th>$ sudo ufw allow port</th></tr></table>

Các port phổ biến

20 ftp-data File Transfer [Default Data]

21 ftp File Transfer [Control]

22 Ssh SSH Remote Login Protocol

23 telnet Telnet

25 Smtp Simple Mail Transfer

53 Domain Domain Name Server

80 www-http World Wide Web HTTP

110 POP3 (non-secure) Post Office Protocol – Version 3

995 POP3 (secure) Post Office Protocol Secure

143 IMAP (non-secure) Internet Message Access Protocol

993 IMAP (secure) Internet Message Access Protocol

465 Smtp (SSL) Simple Mail Transfer with SSL

587 Smtp (TLS) Simple Mail Transfer with TLS

443 https HTTP Secure

<h2>5/ Thay đổi hostname đúng chuẩn FQDN. Ví dụ: sv48d182.domain.ltd (thay thế domain.ltd của bạn)</h2>

Check hostname

<table style="width:100%;height:10%"><tr><th>hostname</th></tr></table>

Thay đổi Hostname

<table style="width:100%;height:10%"><tr><th>hostnamectl set-hostname teammail-sp.xyz</th></tr></table>

<h2>6/ Thay đổi timezone cho đúng muối giờ của VN</h2>

Check date

<table style="width:100%;height:10%"><tr><th>date</th></tr></table>

Thay đổi Date

<table style="width:100%;height:10%"><tr><th>sudo dpkg-reconfigure tzdata</th></tr></table>

<h2>7/ Cài đặt mô hình LEMP</h2>
<h3>1.1 Cài đặt Nginx</h3>

Kiểm tra và cập nhật các gói Packet

Kiểm tra cập nhật các gói 

<table style="width:100%;height:10%"><tr><th>sudo apt-get update && sudo apt-get upgrade -y</th></tr></table>

Cài đặt nginx

<table style="width:100%;height:10%"><tr><th>sudo apt-get install nginx -y</th></tr></table>

Cài đặt xong kiểm tra trạng thái

<table style="width:100%;height:10%"><tr><th>service nginx status (sudo systemct nginx status)</th></tr></table>

chạy lệnh hoạt động 

<table style="width:100%;height:10%"><tr><th>sudo systemctl start nginx</th></tr></table>

Kiểm tra version

<table style="width:100%;height:10%"><tr><th>sudo nginx -y</th></tr></table>

Khởi động nginx hoạt động cùng hệ thống
<table style="width:100%;height:10%"><tr><th>sudo systemctl enable nginx</th></tr></table>
<h3>1.2 Cài đặt MySQL</h3>
<p>MySQL một hệ thống quản trị cơ sở dữ liệu mã nguồn mở phổ biến nhất được sử dụng từ các website lớn tới nhỏ.</p>
<p>Để cài đặt MySQL các bạn thực hiện như theo bên dưới.</p>
<p>Chạy câu lệnh cài đặt MySQL:</p>
<table style="width:100%;height:10%"><tr><th>sudo apt-get install mysql-server -y</th></tr></table>
Kiểm tra user có trong bảng user
<table style="width:100%;height:10%"><tr><th>select user,authentication_string,plugin,host from mysql.user;</th></tr></table>
Khởi tạo user
<table style="width:100%;height:10%"><tr><th>create user 'phatnt3'@'%' identified by 'Phatnt@2022';</th></tr></table>
Gán quyền user
<table style="width:100%;height:10%"><tr><th>GRANT ALL PRIVILEGES ON *.* TO 'phatnt3'@'%';</th></tr></table>
Chấp nhận thay đổi
<table style="width:100%;height:10%"><tr><th>flush privileges;</th></tr></table>
