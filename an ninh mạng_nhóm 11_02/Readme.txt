BÀI TẬP LỚN MÔN QUẢN TRỊ HỆ THỐNG
Đề tài
Cài đặt và cấu hình hệ thống kiểm soát truy cập - Firewall bằng pfSense trên VMware
1. Công cụ sử dụng

- VMware Workstation
- pfSense CE
- Windows 10
- Mạng ảo VMnet8 NAT
- Mạng ảo VMnet1 Host-only

2. Mô hình mạng

 WAN
- Kết nối Internet thông qua VMnet8 NAT
- pfSense tự động nhận IP WAN bằng DHCP

 LAN
- Kết nối nội bộ thông qua VMnet1 Host-only
- pfSense LAN IP: 192.168.1.1/24

Client Windows 10
- IP Address: 192.168.1.20
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
- DNS Server: 8.8.8.8

3. Các bước thực hiện
Cài đặt VMware Workstation
- Tạo máy ảo pfSense
- Tạo máy ảo Windows 10
- Cấu hình Network Adapter

pfSense
- Adapter 1: VMnet8 NAT
- Adapter 2: VMnet1 Host-only

Windows 10
- Adapter: VMnet1 Host-only

Cài đặt pfSense

Các bước cài đặt:
- Boot file ISO pfSense
- Accept
- OK
- Auto (UFS)
- Install
- Reboot

Gán interface:
- WAN = em0
- LAN = em1
4. Cấu hình pfSense

 WAN
- DHCP tự động nhận IP

 LAN
- IP Address: 192.168.1.1
- Subnet: 24

 DHCP LAN
- Tắt DHCP Server

5. Cấu hình Windows 10

Cấu hình IP tĩnh:

- IP Address: 192.168.1.20
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
- DNS Server: 8.8.8.8

6. Cấu hình Firewall
 Rule cho phép IP cụ thể
 Allow Rule
- Action: Pass
- Interface: LAN
- Source: 192.168.1.20
- Destination: any

 Rule chặn toàn bộ LAN
 Block Rule
- Action: Block
- Interface: LAN
- Source: LAN net
- Destination: any

 Rule chặn ICMP
 ICMP Block
- Protocol: ICMP
- Action: Block

Chặn Web
Action: Block 
Protocol: TCP 
Destination port range: 
- From: 80 
- To: 443 


7. Cấu hình NAT
- Outbound NAT: Automatic
- WAN sử dụng VMnet8 NAT
- LAN sử dụng VMnet1 Host-only

8. Kiểm tra hệ thống

Kiểm tra địa chỉ IP
Sử dụng lệnh:
cmd
ipconfig

Kiểm tra kết nối LAN
ping 192.168.1.1

Kiểm tra Internet
ping 8.8.8.8

Web access
Mở Google.com

Kiểm tra log
Status → System Logs → Firewall