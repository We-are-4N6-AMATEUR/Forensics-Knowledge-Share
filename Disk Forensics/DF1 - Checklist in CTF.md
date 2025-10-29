# Checklist for disk forensics in CTF

## 1. Lời nói đầu
Hôm nay do quá rảnh nên mình ngồi nghĩ lại mấy lần chơi CTF ở các giải, thấy bản thân vẫn chưa có một flow check cụ thể nào. Cứ quen check cái nào thì check cái đó trước. Nhiều khi loạn vì không nhớ bản thân đã check qua folder, artifacts đó chưa. Nên nay mình ngồi lại và tạo cái checklist này. Này một phần từ kinh nghiệm cá nhân nên mình không áp vào cụ thể cá nhân nào. Checklist này chỉ tập trung vào disk evidence trên Windows, không thực hiện trên Linux hay các hệ điều hành khác. Ok let's go.

## 2. Checklist 

Bản thân mình thấy trong các giải CTF thì không phải challenge nào cũng tuân theo 1 chuỗi tấn công hay 1 quy trình tấn công cụ thể nào. Nhưng thường thì mình sẽ follow theo các phase trong MITRE ATT@CK Framework. Các bạn có thể tham khảo tại [link này](https://attack.mitre.org/). 

### 2.1 Initial Access 

Theo MITRE ATT&CK, `Initial Access - Truy cập ban đầu` bao gồm các kỹ thuật sử dụng các vectơ đầu vào khác nhau để có được chỗ đứng ban đầu trong mạng. Một số kỹ thuật phổ biến ở phase này mà có thể bạn đã thấy hoặc nghe qua như : Phishing, Valid Accounts, .... hoặc thông qua các lỗ hổng. Với phase này, mình thực hiện check một số mục bao gồm : 

- Lịch sử trình duyệt : 

| Trình duyệt | Đường dẫn | 
| Chrome | C:\Users\<username>\AppData\Local\Google\Chrome\User Data\Default |
| Edge | C:\Users\<username>\AppData\Local\Microsoft\Edge\User Data\Default |
| Firefox | C:\Users\<username>\AppData\Roaming\Mozilla\Firefox\Profiles\<profile folder>\places.sqlite\ | 