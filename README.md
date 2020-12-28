# VEN_System-Project

1. Kiến trúc hệ thống Community - Event infratructure
Sơ đồ hệ thống (kien-truc-ven-1.0.html)
 
Mô tả các module trong hệ thống 
1. Event:
•	Module lấy data đầu vào từ các hệ thống:  
o	
	Kafka: đọc các messages của hệ thống topic noti (khớp lệnh ORS), moevent (giao dịch BO cơ sở), fds_trans_event (khớp lệnh + giao dịch phái sinh), bpm (message xử lý quy trình) 
	Database BODB, FDSDB: Kết nối vào DB lấy data (thông tin khách hàng, môi giới, giao dịch ...) 
	File input (excel, csv, text): Lấy data từ file của user làm manual 
	UserID: lấy thông tin customer info 
•	Format lại định dạng event, ghi vào DB Solr 
•	Đọc event từ DB Solr, Filter, sinh nội dung message và đẩy vào hệ thống Communications
•	Jobs config tự động theo thời gian quét lấy event từ Database, những event nào cần sinh message sẽ đẩy sang communication.
2. Database Solr Event:
•	Database lưu trữ toàn bộ message nhận được từ hệ thống .
3. Communications: 
•	Nhận thông tin gửi message từ event theo các loại message
•	Build nội dung message theo các template có sẵn tương ứng với loại event - nhận từ Event module  
•	Ghi lại nội dung message trong storage - lưu trữ 
•	Job quét các message và đẩy đi theo các channel gồm: email, sms, noti (d-noti) hoặc dùng cho applicartion. Hỗ trợ nhiều kênh phân phối message khác nhau.
4. Database Solr Community: (DB message) 
•	Database lưu trữ toàn bộ message đã được format. Dùng cho truy vấn, hiển thị sau này theo khách hàng hoặc nhân viên
5. dcom-gatewway:
	Open API cho phép kết nối với bên thứ 3
