## mailto

`mailto` 是一种 URI（统一资源标识符）协议，主要用于在 Web 页面中创建电子邮件链接。当用户点击使用 `mailto` 协议的链接时，系统会自动打开默认的电子邮件客户端，并在新邮件窗口中填充预设的收件人地址、主题、正文等信息。格式如下：

```
mailto:email@example.com?subject=Subject&body=Message
```

例如：

```html
<a href="mailto:someone@example.com?subject=Hello%20there&body=This%20is%20a%20test%20email.">Send Email</a>
```

## 类似的协议

除了 `mailto` 协议，还有许多其他 URI 协议用于不同的目的。以下是一些常见的 URI 协议：

1. **http/https**：
   - 用于访问 Web 页面。`http` 是超文本传输协议，`https` 是其安全版本。
   - 例如：`http://www.example.com`，`https://www.example.com`

2. **ftp**：
   - 用于文件传输协议，允许用户在网络上传输文件。
   - 例如：`ftp://ftp.example.com`

3. **file**：
   - 用于访问本地文件系统中的文件。
   - 例如：`file:///C:/path/to/file.txt`

4. **tel**：
   - 用于拨打电话号码，当用户点击链接时，会触发设备上的电话应用程序。
   - 例如：`tel:+1234567890`

5. **sms**：
   - 用于发送短信，当用户点击链接时，会打开短信应用程序。
   - 例如：`sms:+1234567890`

6. **geo**：
   - 用于地理位置，通常用于地图应用程序。
   - 例如：`geo:37.7749,-122.4194`

7. **sip**：
   - 用于会话初始协议，常用于 VoIP（网络电话）应用。
   - 例如：`sip:username@domain.com`

8. **skype**：
   - 用于 Skype 通话或聊天。
   - 例如：`skype:username?call`，`skype:username?chat`

9. **whatsapp**：
   - 用于打开 WhatsApp 聊天。
   - 例如：`whatsapp://send?phone=1234567890`

10. **data**：
    - 用于包含内联数据，例如在 HTML 中嵌入小型图像。
    - 例如：`data:image/png;base64,iVBORw0KGgoAAAANSUhEUg...`