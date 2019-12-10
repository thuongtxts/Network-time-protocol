Network-time-protocol
=====================


Xin chào các bạn. Hôm nay tôi sẽ về về vấn đề network time protocol. Một nội dung khá quan trọng đối với một hệ thống. Tôi hi vọng sẽ giúp các bạn nắm được các vấn đề cơ bản trong network time protocol.

- [Mục lục](#undefined)
			
- [1. Giới thiệu](#user-content-1-gi%E1%BB%9Bi-thi%E1%BB%87u)
			
- [2. Phương thức hoạt động](#user-content-2-ph%C6%B0%C6%A1ng-th%E1%BB%A9c-ho%E1%BA%A1t-%C4%91%E1%BB%99ng)
			
- [3. Cài đặt](#user-content-3-c%C3%A0i-%C4%91%E1%BA%B7t)
					
     - [a. Chuẩn bị:](#user-content-a-chu%E1%BA%A9n-b%E1%BB%8B)
					
     - [b. Các bước cài đặt](#user-content-b-c%C3%A1c-b%C6%B0%E1%BB%9Bc-c%C3%A0i-%C4%91%E1%BA%B7t)
			
- [4. Mô hình LAB](#user-content-4-m%C3%B4-h%C3%ACnh-lab)
			
- [5. Các câu lệnh đổi thời gian](#user-content-5-c%C3%A1c-c%C3%A2u-l%E1%BB%87nh-%C4%91%E1%BB%95i-th%E1%BB%9Di-gian)
					
     - [a. Thay đổi thời gian của hệ thống:](#user-content-a-thay-%C4%91%E1%BB%95i-th%E1%BB%9Di-gian-c%E1%BB%A7a-h%E1%BB%87-th%E1%BB%91ng)
					
     - [b. Tăng thêm thời gian cho hệ thống](#user-content-b-t%C4%83ng-th%C3%AAm-th%E1%BB%9Di-gian-cho-h%E1%BB%87-th%E1%BB%91ng)
				 
     - [c. Các tùy chọn trong câu lệnh date](#user-content-c-c%C3%A1c-t%C3%B9y-ch%E1%BB%8Dn-trong-c%C3%A2u-l%E1%BB%87nh-date)
 

====================

#### 1. Giới thiệu

Để một hệ thống hoạt động và có thể phối hợp được với nhau điều kiện tiên quyết đầu tiên là giữa các hệ thống đó phải đồng bộ được về mặt thời gian. Nhờ vậy mà các bộ phận điều khiển có thể điều kiển được các bộ phận khác. Khi một hệ thống mất đồng bộ về mặt thời ngay lập tức hệ thống đó sẽ không hoạt động được nữa.

Trong hệ thống máy tính đã sử dụng một giao thức có thể động bộ về mặt thời gian giữa các máy tính. Chính là giao thức Network time protocol (ntp).  Giao thức đồng bộ thời gian mạng ntp là một giao thức để đồng bộ đồng hồ của các hệ thống máy tính thông qua mạng dữ liệu chuyển mạch gói với độ trễ biến đổi. Giao thức này được thiết kế để tránh ảnh hưởng của độ trễ biến đổi bằng cách sử dụng bộ đệm jitter.

#### 2. Phương thức hoạt động
- NTP client gửi một gói tin, trong đó chứa một thẻ thời gian tới cho NTP server.
- NTP server nhận được gói tin, gửi trả lại NTP client một gói tin khác, có thẻ thời gian là thời điểm nó gửi gói tin đó đi.
- NTP client nhận được gói tin đó, tính toán độ trễ, dựa và thẻ thời gian mà nó nhận được cùng với độ trễ đường truyền, NTP client sẽ set lại thời gian của nó.

#### 3. Cài đặt
###### a. Chuẩn bị:
Chuẩn bị 2 máy cài centos và được kết nối với nhau như trong mô hình sau
<img class="image__pic js-image-pic" src="http://i.imgur.com/YqfoT1S.png" alt="" id="screenshot-image">
###### b. Các bước cài đặt
- Bước 1: Tải và cài đặt gói ntp
```
# yum -y install ntp 
```
Đợi cho quá trình cài đặt thành công

- Bước 2 Cấu hình cho máy chủ ntp
Vào chỉnh sử file ntp.conf
```
vi /etc/ntp.conf
```
<img class="image__pic js-image-pic" src="http://i.imgur.com/4T7BosR.png" alt="" id="screenshot-image">
```
Khi bạn không muốn sử dung đồng bộ theo thời gian của máy chủ centos. Thì chỉ việc xóa dòng các máy chủ của mình vào bước này sử dụng cho cấu hình ntp-client
```
Quá trình cài đặt gói phần mềm ntp đã xong. Do ntp hoạt động trên nền giao thức upd với port 123 nên ta cần mở port đối với máy server và client
      
  Các cách mở port:
      
- Đối với những bạn không hiểu nhiều về iptables trong Centos. Các bạn có thể tắt iptables đi bằng sử dụng lệnh
      ```
      # service iptables stop
      ```
      
- Đối với những bạn đã hiểu về iptables có thể dùng lênh sau để mở port
      ```
      #iptables -A INPUT -p udp --dport 123 -j ACCEPT
      #iptables -A OUTPUT -p udp --sport 123 -j ACCEPT
      ```

- Để ntp luôn được bật lên khi khởi động bạn sử dụng câu lệnh
```
# chkconfig ntpd on
```
- Bước 3 : Khởi động lại dịch vụ ntp
```
# service ntp restart
```
#### 4. Mô hình LAB
Các bạn có thể xem mô hình LAB sau

<img class="image__pic js-image-pic" src="http://i.imgur.com/4T7BosR.png" alt="" id="screenshot-image">

Quá trình Lab trên máy client là Linux

<img class="image__pic js-image-pic" src="http://i.imgur.com/yu5RSFn.png" alt="" id="screenshot-image">

- Câu lệnh để đồng bộ thời gian với máy chủ ngay lập tức
```
# ntpdate -u [địa chỉ ip của ntp server]
```

Quá trình Lab trên máy client là Windown.

Các bạn thức hiện theo các bước sau:
<img class="image__pic js-image-pic" src="http://i.imgur.com/gBLoFeo.png" alt="" id="screenshot-image">

<img class="image__pic js-image-pic" src="http://i.imgur.com/omLsuJt.png" alt="" id="screenshot-image">

<img class="image__pic js-image-pic" src="http://i.imgur.com/msWdzaN.png" alt="" id="screenshot-image">

#### 5. Các câu lệnh đổi thời gian

Đầu tiên bạn hay tắt dịch vụ ntp đi bằng cách sử dụng câu lệnh
```
# service ntpd stop
```
###### a. Thay đổi thời gian của hệ thống:
Cách 1:
```
# date +'FORMAT' -s 'thời gian'
```
VD: #date + '%Y%m%d %H%M' -s'20141009 2005'

<img class="image__pic js-image-pic" src="http://i.imgur.com/jjvAZT6.png" alt="" id="screenshot-image">

Cách 2: Các bạn có thể dùng sử dụng đúng định dạng của hệ thống 
```
#date -s '2014-09-10 20:04:222'
```
<img class="image__pic js-image-pic" src="http://i.imgur.com/Kn7JETJ.png" alt="" id="screenshot-image">

###### b. Tăng thêm thời gian cho hệ thống

Các bạn dùng câu lệnh date -s rồi thêm lương gian cần thiết có thể là giờ, phút,giây
```
date -s '<khoảng thời gian muốn tăng thêm> [seconds,minutes, hours]'
```

VD:

<img class="image__pic js-image-pic" src="http://i.imgur.com/hAZTiPR.png" alt="" id="screenshot-image">
các bạn làm tương tự với giờ và phút

###### c. Các tùy chọn trong câu lệnh date

|%H | Lựa chọn giờ |
|---|--------------|
|%M | Lựa chọn phút|
|%Y | Hiển thị năm  |
|%m | Hiển thị tháng |
|%F | Hiển thị thông tin đầy đủ về thời gian |
|%d | Hiển thị ngày |

Các bạn có thể xem thêm các tùy chọn của lệnh date tại đây

[ Chi tiết về date](http://linux.about.com/od/commands/l/blcmdl1_date.htm)

*Kết luận:* Quá trình đồng bộ thời gian giữa các máy là công việc đầu tiên và quan trong đối với một hệ thống cần phối hợp với nhau nên bạn cần nắm vững nội dung về ntp. 

Các bạn có thể xem thêm link tham khảo tại đây

[Link tham khảo 1](http://www.tecmint.com/install-ntp-server-in-centos/)

[Link tham khảo 2](http://ask.xmodulo.com/change-date-time-command-line-linux.html)

[Link tham khảo 3](http://www.gocit.vn/bai-viet/network-time-protocol-ntp-tren-linux/)

Người thực hiện: Nguyễn Hoài Nam
skype: namptit307



