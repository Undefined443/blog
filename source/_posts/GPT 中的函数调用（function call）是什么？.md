在 OpenAI ChatGPT API 和 Google Gemini API 中我们可以看到函数调用的功能。这个功能是做什么用的？下面大概讲解。

以 [Google Gemini API 函数调用](https://ai.google.dev/tutorials/function_calling_python_quickstart) 一节中的内容为例，该章节举了一个例子：

大语言模型（LLMs）往往无法进行准确的数学运算。比如说，给 Gemini 两个数 $a$ 和 $b$，让它计算 $a \times b$ 的值。Gemini 给出的值往往和实际计算值有所出入。

```py
model = genai.GenerativeModel('gemini-pro')
chat = model.start_chat()

a = 2312371
b = 234234

response = chat.send_message(
    f"What's {a} X {b} ?"
)
print(response.text)
```

Gemini 给出的值是：

```
549899573314
```

而 $a \times b$ 的实际值是 `541635908814`，Gemini 的计算并不正确。

我们会想：既然 Gemini 算不正确，但是这种小事我们用 Python 就能算正确了呀。能不能让 Gemini 像我们一样使用 Python 来进行计算呢？

答案是可以。

只要我们在本地定义好 Gemini 需要调用的函数，再向 Gemini 声明我们为它定义了哪些函数、函数的功能是什么，以及函数的参数有哪些，Gemini 就可以在它认为需要调用这些函数的时候，以对话的形式向我们请求调用这些函数。

比如说，我们希望 Gemini 的乘法运算能更准确一点，于是我们为它定义一个乘法函数：

```py
def multiply(a, b):
    return a * b
```

我们还要告诉 Gemini 我们为它定义了 `multiply` 这个函数，用于计算两个数的乘积，并且它需要两个数字参数 `a` 和 `b`：

```py
calculator = glm.Tool(
    function_declarations=[
      # 在这里进行函数声明
      glm.FunctionDeclaration(
        name='multiply',  # 函数名
        description="Returns the product of two numbers.",  # 功能描述
        # 在这里进行参数声明
        parameters=glm.Schema(
            type=glm.Type.OBJECT,
            properties={
                # 需要两个参数 a 和 b，类型为 NUMBER
                'a':glm.Schema(type=glm.Type.NUMBER),
                'b':glm.Schema(type=glm.Type.NUMBER)
            },
            required=['a','b']  # 声明必要参数，这里 a 和 b 都是必要的
        )
      )
    ])
```

这样，Gemini 就知道了我们为它准备了一个函数 `multiply`，用于乘法运算。

现在，我们再次询问 Gemini $a \times b$ 的值：

```py
model = genai.GenerativeModel('gemini-pro', tools=[calculator])
chat = model.start_chat()

a = 2312371
b = 234234

response = chat.send_message(
    f"What's {a} X {b} ?",
)
```

Gemini 在分析完我们的问题之后发现它要计算 $a \times b$ 的值，并且还发现我们已经为它定义了一个函数 `multiply` 用于乘法运算。所以，它在生成回答之前，会先向我们请求调用 `multiply` 这个函数。我们在 `response.candidates` 这个属性中可以看到它请求调用的函数名：

```py
response.candidates
```

```py
[index: 0
content {
  parts {
    function_call {
      name: "multiply"
      args {
        fields {
          key: "b"
          value {
            number_value: 234234
          }
        }
        fields {
          key: "a"
          value {
            number_value: 2312371
          }
        }
      }
    }
  }
  role: "model"
}
finish_reason: STOP
]
```

可以看到，在 `function_call` 属性中，它请求了 `multiply` 这个函数，并且给出了函数的参数 `a` 和 `b`。

接下来，我们只需在本地为它执行 `multiply` 这个函数，并把函数的返回值传递给它即可：

```py
fc = response.candidates[0].content.parts[0].function_call  # 获取 Gemini 请求的函数调用相关信息
if fc.name == 'multiply':  # 判断函数调用的名字是否为 multiply
    result = mutiply(fc.args['a'], fc.args['b'])  # 执行函数调用

# 最后，将函数调用的结果传递给 Gemini
response = chat.send_message(
    glm.Content(
    parts=[glm.Part(
        # 在这里传入函数调用的结果
        function_response = glm.FunctionResponse(
          name='multiply',
          response={'result': result}
        )
    )]
  )
)
```
