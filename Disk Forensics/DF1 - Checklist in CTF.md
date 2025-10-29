# Checklist for disk forensics in CTF

## 1. Lời nói đầu
Hôm nay do quá rảnh nên mình ngồi nghĩ lại mấy lần chơi CTF ở các giải, thấy bản thân vẫn chưa có một flow check cụ thể nào. Cứ quen check cái nào thì check cái đó trước. Nhiều khi loạn vì không nhớ bản thân đã check qua folder, artifacts đó chưa. Nên nay mình ngồi lại và tạo cái checklist này. Này một phần từ kinh nghiệm cá nhân nên mình không áp vào cụ thể cá nhân nào. Checklist này chỉ tập trung vào disk evidence trên Windows, không thực hiện trên Linux hay các hệ điều hành khác. Ok let's go.

## 2. Checklist 

Bản thân mình thấy trong các giải CTF thì không phải challenge nào cũng tuân theo 1 chuỗi tấn công hay 1 quy trình tấn công cụ thể nào. Nhưng thường thì mình sẽ follow theo các phase trong MITRE ATT@CK Framework. Các bạn có thể tham khảo tại [link này](https://attack.mitre.org/). Tất nhiên là ta cũng phải theo yêu cầu đề bài là dạng tìm flag hay theo dạng trả lời câu hỏi để nhận flag. 

### 2.1 Initial Access 

Theo MITRE ATT&CK, `Initial Access - Truy cập ban đầu` bao gồm các kỹ thuật sử dụng các vectơ đầu vào khác nhau để có được chỗ đứng ban đầu trong mạng. Một số kỹ thuật phổ biến ở phase này mà có thể bạn đã thấy hoặc nghe qua như: Phishing, Valid Accounts, .... hoặc thông qua các lỗ hổng. Với phase này, mình thực hiện check một số mục bao gồm:

#### 2.1.1 Lịch sử trình duyệt 

Ta cần kiểm tra xem nạn nhân có tải xuống file nào đáng ngờ không ? Có truy cập đường dẫn, liên kết đáng ngờ nào không ? Cơ bản trước là vậy. 

| Trình duyệt | Đường dẫn | 
| ------------- | ------------- |
| Chrome | C:\Users\<username>\AppData\Local\Google\Chrome\User Data\Default |
| Edge | C:\Users\<username>\AppData\Local\Microsoft\Edge\User Data\Default |
| Firefox | C:\Users\<username>\AppData\Roaming\Mozilla\Firefox\Profiles\<profile folder>\places.sqlite | 

Có thể attacker hay người ra đề có thể xóa phần này để làm tăng độ khó của bài. 

Công cụ sử dụng:
- `DB Browser`: Để mở file các file mình đã đề cập ở trên. Các file này thường là file database,

#### 2.1.2 Event log

Mình thường check 2 Event ID là 4624 - Login Successful và 4625 - Login Failed. Nếu đề bài liên quan đến AD thì có thể mở rộng ra event ID như:

| Event ID | ý nghĩa| 
| ------------- | ------------- |
| 4672 | Special privileges assigned to new logon | 
| 4771 | Kerberos pre-authentication failed |
| 4776 | NTLM authentication failed |

Phần này nhằm mục đích kiểm tra các user có đăng nhập từ các địa chỉ IP lạ nào không ? Người dùng có đặc quyền cao và đăng nhập từ địa chỉ IP lạ ? Login failed nhiều lần, xác thực Kerberos,NTLM failed nhiều lần -> dấu hiệu của tấn công Brute Force.  

Công cụ sử dụng: 
- `EvtxeCmd`: Chuyển evtx thành các file csv hoặc json.
- `Timeline Explorer`: Sử dụng để đọc trực quan các file csv hoặc json.

*Bổ sung:*

Ở phần này ta có thể xem qua các thư mục của Users như Downloads, Documents, Desktop.... Nhớ là xem qua và không nên tập trung quá nhiều thời gian vào lục lọi các thư mục làm gì nếu không có hướng làm từ đầu.

### 2.2 Execution 

`Execution - thực thi` về cơ bản là thực thi các đoạn mã, command bởi attacker hoặc người ra đề (bối cảnh CTF). Với phần này, ta có thể tập trung vào một số evidence như: 

#### 2.2.1 Event log

Có khá nhiều event log có thể cho ta bằng chứng về việc chạy mã, command bởi attacker hoặc author. 

| Event ID | Ý nghĩa | 
| ------------- | ------------- |
| 800 | Pipeline execution details | 
| 4104 | Powershell Script Block Logging | 
| 4103 | Module logging | 
| 4100 | Executing pipeline | 

Với EID 800 có khá nhiều sự kiện cũng sử dụng EID này nên ta cần lưu ý là phải là pipeline execution. Các EID này sẽ cho ta cái nhìn cơ bản, thậm chí là sâu để tìm ra nguyên nhân cuộc tấn công, các lệnh được thực thi.... Tất nhiên là nó sẽ chỉ có tác dụng nếu nó không bị xóa :)))

Công cụ sử dụng: 
- `EvtxeCmd`: Chuyển evtx thành các file csv hoặc json.
- `Timeline Explorer`: Sử dụng để đọc trực quan các file csv hoặc json.

#### 2.2.2 Console History

File này nằm ở đường dẫn `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine`. 







