Selenium 用于控制浏览器完成一系列自动化操作。

## 安装

```sh
pip install selenium
```

还需安装浏览器驱动：

- 对于 Chrome，需要安装 [ChromeDriver](https://googlechromelabs.github.io/chrome-for-testing/)。
- 对于 Firefox，需要安装 [GeckoDriver](https://github.com/mozilla/geckodriver/releases)。

ChromeDriver 和 GeckoDriver 可以通过包管理器 APT 或者 Homebrew 安装。

## 示例

使用 Google 搜索“Selenium”关键词并在命令行输出找到的结果：

```python
from time import sleep
from selenium import webdriver
from selenium.webdriver.common.by import By

# 1. 使用驱动实例开启会话
driver = webdriver.Chrome()

# 2. 在浏览器上执行操作
driver.get("https://google.com") # 导航到一个网页
sleep(1)

# 3. 请求浏览器信息
title = driver.title
print(f"Title: {title}")

# 4. 建立等待策略
driver.implicitly_wait(0.5) # 如果在指定时间内没有找到元素，则抛出 NoSuchElementException 异常。

# 5. 发送命令查找元素
text_box = driver.find_element(by=By.CSS_SELECTOR, value="textarea") # 通过 CSS 选择器查找元素

# 6. 操作元素
text_box.send_keys("Selenium")
sleep(1)
text_box.submit()
sleep(1)

# 7. 获取元素信息
answers = driver.find_elements(by=By.TAG_NAME, value="h3") # 通过标签名查找元素
print("Answers:")
for i, answer in enumerate(answers):
    print(f"{i + 1}. {answer.text}")

# 8. 结束会话
driver.quit()
```

参考：

- [编写第一个 Selenium 脚本 | Selenium](https://www.selenium.dev/zh-cn/documentation/webdriver/getting_started/first_script/)

See also:

- [Selenium 文档](https://www.selenium.dev/zh-cn/documentation/)
- [等待 | Selenium](https://www.selenium.dev/zh-cn/documentation/webdriver/waits/)
- [导航 | Selenium](https://www.selenium.dev/zh-cn/documentation/webdriver/interactions/navigation/)
- [操作 | Selenium](https://www.selenium.dev/zh-cn/documentation/webdriver/elements/interactions/)

## Troubleshooting

无法启动 Chrome:

```

Traceback (most recent call last):

  File "/Users/xiao/Library/Mobile Documents/com~apple~CloudDocs/Projects/PycharmProjects/ScoreCollector/计科前5学期成绩查询.py", line 32, in <module>

    driver = WebDriver(service=service)

             ^^^^^^^^^^^^^^^^^^^^^^^^^^

  File "/Users/xiao/.local/share/virtualenvs/ScoreCollector-5YizM67E/lib/python3.11/site-packages/selenium/webdriver/chrome/webdriver.py", line 80, in __init__

    super().__init__(

  File "/Users/xiao/.local/share/virtualenvs/ScoreCollector-5YizM67E/lib/python3.11/site-packages/selenium/webdriver/chromium/webdriver.py", line 104, in __init__

    super().__init__(

  File "/Users/xiao/.local/share/virtualenvs/ScoreCollector-5YizM67E/lib/python3.11/site-packages/selenium/webdriver/remote/webdriver.py", line 286, in __init__

    self.start_session(capabilities, browser_profile)

  File "/Users/xiao/.local/share/virtualenvs/ScoreCollector-5YizM67E/lib/python3.11/site-packages/selenium/webdriver/remote/webdriver.py", line 378, in start_session

    response = self.execute(Command.NEW_SESSION, parameters)

               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  File "/Users/xiao/.local/share/virtualenvs/ScoreCollector-5YizM67E/lib/python3.11/site-packages/selenium/webdriver/remote/webdriver.py", line 440, in execute

    self.error_handler.check_response(response)

  File "/Users/xiao/.local/share/virtualenvs/ScoreCollector-5YizM67E/lib/python3.11/site-packages/selenium/webdriver/remote/errorhandler.py", line 209, in check_response

    raise exception_class(value)

selenium.common.exceptions.WebDriverException: Message:

```

解决方法：关闭代理😂

[\[🐛 Bug\]:Python upgrading to selenium 4 raises exception: selenium.common.exceptions.WebDriverException: Message: #10710](https://github.com/SeleniumHQ/selenium/issues/10710)