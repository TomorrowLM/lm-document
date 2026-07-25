# 字符串

- **第一种：普通字符串** [02:03](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=123)

  - 使用单引号或双引号括起字符串内容。
  - 示例：`print("Hello")` 或 `print('World')`

- **第二种：原始字符串** [02:29](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=149)

  - 使用前缀 `r` 表示原始字符串。
  - 不将反斜杠视为转义字符。
  - 示例：`print(r"C:\new\text.txt")` 输出 `C:\new\text.txt`
  - 应用于路径、正则表达式等需要保留反斜杠的场景。

- **第三种：三引号多行字符串** [05:13](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=313)

  - 使用三个单引号或双引号括起内容。

  - 支持换行、包含引号。

  - 示例：

    ```python
    print('''一二三
    四五六''')
    ```

  - 可用于多行文本、文档字符串（docstring）。

- **第四种：格式化字符串（f-string）** [07:39](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=459)

  - 使用前缀 `f` 或 `F`。

  - 在字符串中使用花括号 `{}` 插入变量或表达式。

  - 示例：

    python

    ```python
    name = "Tom"
    print(f"Hello, {name}")
    ```

  - 常用于动态输出信息，提升代码可读性。

- **第五种：Unicode字符串** [13:14](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=794)

  - 使用前缀 `u` 表示Unicode字符串。
  - 示例：`u"你好"`
  - 用于处理非ASCII字符，避免文件编码问题。
  - 推荐配合标准库如 `codecs` 使用。

- **第六种：字节串（bytes）** [15:54](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=954)

  - 使用前缀 `b` 表示字节串。

  - 示例：`b"hello"`

  - 表示二进制数据。

  - 常用于网络传输、文件操作中字节流处理。

  - 示例操作：

    python

    ```python
    data = b"hello"
    decoded = data.decode("utf-8")
    encoded = decoded.encode("utf-8")
    ```

- **Python与Java、JavaScript字符串对比** [18:12](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=1092)

  - **定义方式**
    - Python：支持单引号、双引号、三引号。
    - Java：字符串用双引号表示，字符用单引号。
    - JavaScript：单引号、双引号均可，反斜杠用于换行。
  - **不可变性**
    - Python 和 Java 的字符串对象不可变，修改会创建新对象。
    - JavaScript 同样不可变。
    - Vue.js 等框架中变量可变，但不是字符串本身的特性。
  - **字符串方法**
    - Python 提供丰富内置方法（如 `split`, `replace`, `join` 等）。
    - Java 和 JavaScript 方法类似，但命名和参数略有不同。
    - 示例对比：
      - Python: `s.split()`
      - Java: `s.split("\\s+")`
      - JavaScript: `s.split(/\s+/)`
  - **格式化支持**
    - Python 有 f-string。
    - Java 使用 `String.format()` 或 `Formatter`类。
    - JavaScript 使用模板字符串（反引号 + `${}`）。
  - **多行支持**
    - Python 使用三引号。
    - JavaScript 使用反引号（`）。
    - Java 需手动拼接或使用 `\n`。

- **总结与建议** [26:07](https://b.quark.cn/apps/5AZ7aRopS/routes/mofb35Rkb?debug=0&fid=ef717e6f379b473b81a6def3c9cbf6c1#?seek_t=1567)

  - 掌握各种字符串形式的写法和适用场景。
  - 多加练习格式化字符串和三引号的使用。
  - 注意 Unicode 编码处理，避免乱码问题。
  - 对比不同语言字符串特性，提升跨语言开发能力。