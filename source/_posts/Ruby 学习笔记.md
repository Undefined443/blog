## 基本语法

### 变量

```ruby
name = "Alice"
age = 30
puts "Name: #{name}, Age: #{age}"
```

```ruby
var  # 局部变量
@var # 实例变量
$var # 全局变量
```

### 数据类型

Ruby 支持多种数据类型，包括字符串、数字、数组、哈希等。

```ruby
# 字符串
str = "Hello, Ruby!"

# 数字
num = 42

# 数组
arr = [1, 2, 3, 4, 5]

# 哈希
hash = {name: "Alice", age: 30}
```

### 条件语句

```ruby
age = 18

if age < 18
  puts "You are a minor."
elsif age >= 18 && age < 65
  puts "You are an adult."
else
  puts "You are a senior."
end
```

### 循环

```ruby
# while 循环
i = 0
while i < 5
  puts "i is #{i}"
  i += 1
end

# each 循环
arr = [1, 2, 3, 4, 5]
arr.each do |num|
  puts num
end
```

### 方法

```ruby
def greet(name)
  return "Hello, #{name}!"
end

puts greet("Alice")
```

### 类和对象

```ruby
class Person
  attr_accessor :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end

  def introduce
    "Hello, my name is #{@name} and I am #{@age} years old."
  end
end

person = Person.new("Alice", 30)
puts person.introduce
```

## 安装

macOS:

```sh
brew install ruby
```

Ubuntu:

```sh
sudo apt install ruby
```

## 运行

将你的 Ruby 代码保存到一个 `.rb` 文件中，例如 `hello.rb`。然后在终端中运行：

```sh
ruby hello.rb
```

## 包管理器 Gem

Gem 是 Ruby 编程语言的包管理工具，类似于 Python 的 pip 或者 JavaScript 的 npm。

### 基本命令

```sh
gem install <gem_name>   # 安装 Gem 包
gem list                 # 列出已安装的 Gem 包
gem search <gem_name>    # 搜索 Gem 包
gem update <gem_name>    # 更新 Gem 包
gem uninstall <gem_name> # 卸载 Gem 包
gem info <gem_name>      # 查看 Gem 包信息
```

查看 Gem 包文档：

```sh
gem server
```

运行此命令后，打开浏览器并访问 http://localhost:8808，可以查看本地已安装 Gem 包的文档。

### Bundler

在实际项目中，我们通常使用 Bundler 来管理 Gem 依赖项。Bundler 使用 `Gemfile` 文件来定义项目所需的 Gem 以及它们的版本。

安装 Bundler：

```sh
gem install bundler
```

在项目根目录下创建一个名为 `Gemfile` 的文件，内容如下：

```ruby
source 'https://rubygems.org'

gem 'rails', '~> 6.1.0'
gem 'pg', '>= 0.18', '< 2.0'
```

安装 `Gemfile` 中指定的 Gem：

```sh
bundle install
```

更新 `Gemfile` 中指定的 Gem：

```sh
bundle update
```

## 版本管理器

Ruby 的主流版本管理器有 RVM 和 rbenv。我还没有遇到需要使用版本管理器的情况，因此这里留空。