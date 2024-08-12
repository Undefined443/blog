在编写 Python 脚本时，我们会使用一些私密数据，如调用 API 时使用的 token。为了避免隐私泄露，这些私密数据一般不直接写入脚本文件中。而是写入一个文件，并通过读取文件的方式获取私密数据内容。这个文件就是 `.env` 文件。

`.env` 文件中以 `KEY=VALUE` 的形式存储着我们的私密数据。比如：

```sh
OPENAI_API_KEY=sk-sfdsfw9r9erthgsyorwtogsgjft
# 支持写注释
GOOGLE_API_KEY=jGHODSFljdsfsodhfsdjgshgoihsdgioh
```

安装依赖 [Python Dotenv](https://pypi.org/project/python-dotenv/)：

```sh
pip install python-dotenv
```

在 Python 脚本中读取这些环境变量：

```python
from os import environ  # environ 用于获取系统环境变量
from dotenv import load_dotenv  # load_dotenv 用于将 .env 文件的内容加载到系统环境变量中

load_dotenv()  # 从程序所在目录的 .env 文件中加载环境变量

OPENAI_API_KEY=environ.get("OPENAI_API_KEY")  # 获取环境变量 OPENAI_API_KEY
```