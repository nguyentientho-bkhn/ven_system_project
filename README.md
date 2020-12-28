# VEN_System-Project
 
I) Mô tả các module trong hệ thống 
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


II) Cấu trúc code từng module:
1. Module code D-Event
No	Module	Description
1>	vnds-event:	Module parent
2>	vnds-event-app: -	Module frontend sử dụng công nghệ jsp làm giao diện cho quản trị admin của VEN
                  - Chứa các API của VEN cho phép các service nội bộ kết nối
3>	vnds-event-message:	Module build các message tập trung
4>	vnds-event-model:	Module định nghĩa các object mapping với cơ sở dữ liệu solr
5>	vnds-event-service:	Module xử lý logic build message
6>	vnds-event-source:	Module xử lý các connection với các nguồn dữ liệu đầu vào
7>	vnds-event-storage:	Module thực hiện lưu trữ dữ liệu vào solr

2. Module code D-communication
No	Module	Description
1>	communication-frontend-api:	Module chứa các API của D-com
2>	communication-frontend-model:	Module chứa các object request đến d-com
3>	communication-internal-common:	Module chứa các abstract object và utils
4>	communication-internal-service:	Module chứa logic xử lý các message để phân phối đến các kênh theo nghiệp vụ
5>	communication-internal-storage:	Module thực hiện lưu trữ dữ liệu vào solr
6>	d-communication:	Module parent


