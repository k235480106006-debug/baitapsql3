# BÀI TẬP VỀ NHÀ 03: THIẾT KẾ VÀ CÀI ĐẶT CSDL QUẢN LÝ CẦM ĐỒ

# Môn học: Hệ quản trị CSDL. Lớp 59KMT. GV: Đỗ Duy Cốp

# Họ và Tên: NGUYỄN VĂN CƯƠNG

# MSSV:K235480106006

# LỚP: K59KMT

# Nhiệm vụ 1: Thiết kế CSDL
Vẽ sơ đồ ERD: thể hiện rõ thực thể, thuộc tính, khóa chính, khóa ngoại.

Chuyển sơ đồ thành các bảng (Lưu ý chuẩn hóa tối thiểu mức 3NF).

Hệ thống quản lý cầm cố/thế chấp cần xử lý:

- Quản lý khách hàng

- Quản lý hợp đồng vay

- Một hợp đồng có nhiều tài sản thế chấp

- Theo dõi biến động khoản nợ theo thời gian

- Theo dõi trạng thái hợp đồng

Để đạt chuẩn hóa tối thiểu 3NF, cần tách:
- Thông tin khách hàng

- Thông tin hợp đồng

- Thông tin tài sản

- Quan hệ hợp đồng và tài sản

- Lịch sử biến động nợ/trạng thái

  <img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/dbf4e15f-f899-4e40-b1c6-9e2184e17300" />

Nhiệm vụ 2: Cài đặt SQL (Yêu cầu viết Scripts)
Event 1: Đăng ký hợp đồng mới (Vay tiền)
Viết Store Procedure tiếp nhận hợp đồng: Lưu thông tin khách hàng, danh sách tài sản

(kèm giá trị định giá), số tiền vay gốc và thiết lập 2 mốc Deadline1, Deadline2.

Tạo Store Procedure tiếp nhận hợp đồng

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/f00f1e36-a715-40bc-8019-d8bf119c7716" />

sau khi đã tạo Store Procedure xong, test dữ liệu và ta được:

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/19daa8cf-fd3e-43fd-88cc-c36482ec7465" />

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/411e3da5-22ae-4591-886f-f60d3fed4dba" />

Event 2: Tính toán công nợ thời gian thực
Viết một Function fn_CalcMoneyTransaction(TransactionID, TargetDate) để tính số tiền phải trả của TransactionID này cho đến ngày TargetDate

Viết một Function fn_CalcMoneyContract(ContractID, TargetDate) để tính tổng số tiền khách(ContractID) phải trả (Gốc + Lãi đơn + Lãi kép) tính đến ngày TargetDate.

  <img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/03708056-6476-4490-a0e2-29698ca07dfe" />

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/2cc8b35b-bec1-4d40-ac1e-a0fc11501aee" />

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/b77d806b-2634-415a-a638-3606bae65274" />

Event 3: Xử lý trả nợ và hoàn trả tài sản
Viết Viết Store Procedure xử lý khi khách mang tiền đến:

- Nếu tài sản đã bị thanh lý (sau Deadline 2 và có cờ IsSold): Thông báo không thu tiền, không trả đồ.

- Nếu tài sản chưa bị thanh lý: Tính tổng nợ, trừ số tiền khách trả vào hệ thống. Nếu trả hết tiền, trả hết đồ và cập nhật trạng thái hợp đồng thành “Đã thanh toán đủ”; Nếu chưa trả hết tiền gốc+lãi: cập nhật trạng thái hợp đồng thành “Đang trả góp”, ghi nhận vào LOG số tiền đã trả, và số tiền còn nợ.

Đưa ra danh sách gợi ý trả lại cho khách hàng này dựa trên điều kiện:
- Giá trị tài sản còn lại >= Dư nợ còn lại

sau khi đã tạo và test kết quả hiển thị:

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/1e0c0047-10a2-4ab8-8289-ce885845cb3d" />

test bảng hiển thị khách hàng đã trả nợ và còn nợ

<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/305be38d-ae43-4481-a8fe-7edafc43f535" />

Procedure SP_TRA_NO giúp tự động hóa quy trình xử lý trả nợ trong hệ thống cầm đồ.

Hệ thống có thể:

kiểm soát công nợ

xử lý thanh toán

cập nhật trạng thái hợp đồng

quản lý tài sản thế chấp

một cách chính xác và an toàn hơn so với xử lý thủ công.

Event 4: Truy vấn danh sách nợ xấu (Nợ khó đòi)
Xuất danh sách các khách hàng đã quá Deadline 1 mà chưa thanh toán.

Yêu cầu các cột: Tên KH, Số điện thoại, Số tiền vay gốc, Số ngày quá hạn, Tổng tiền phải trả hiện tại (đến ngày hiện tại), Tổng số tiền phải trả sau 1 tháng nữa.

Chức năng này dùng để thống kê danh sách khách hàng có nguy cơ trở thành nợ xấu hoặc đã quá hạn thanh toán.

Hệ thống sẽ hiển thị:

tên khách hàng

số điện thoại

số tiền vay gốc

số ngày quá hạn

tổng số tiền phải trả hiện tại

tổng số tiền phải trả sau 1 tháng nữa

Nhờ đó nhân viên có thể:

theo dõi công nợ

nhắc khách thanh toán

đánh giá khả năng thu hồi nợ

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/3cbcd267-090b-4d70-822c-3a3222da0bf1" />

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/5522fa04-9c16-48db-8bf0-71f719ad7f27" />

Chức năng này giúp:

quản lý danh sách nợ khó đòi

theo dõi khách hàng quá hạn

hỗ trợ nhắc nợ

đánh giá rủi ro tài chính

hỗ trợ quyết định thanh lý tài sản

<img width="1274" height="799" alt="image" src="https://github.com/user-attachments/assets/e2ada6d1-3839-47fc-b170-8686b4ddba54" />

<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/66b5d2e1-44d1-431c-af66-be493c231d6f" />

Trigger giúp:

đồng bộ dữ liệu hợp đồng và tài sản

xác nhận tài sản đã bán

tránh trả nhầm tài sản cho khách

Event 5 giúp hệ thống tự động quản lý quy trình nợ xấu và thanh lý tài sản bằng Trigger.
