# 标题
<details>
<summary></summary>

# 一级标题
    # 一级标题
## 二级标题
    ## 二级标题
### 三级标题
    ### 三级标题
#### 四级标题
    #### 四级标题
##### 五级标题
    ##### 五级标题
###### 六级标题
    ###### 六级标题
</details>

# 分割线

<details>
<summary></summary>
三个星号（*）/连字符（-）/下划线（_）可创建分割线
</details>

# 分段
<details>
<summary></summary>
这是一个段落。
这不是一个段落。  
（上行末尾有两个以上空格）这也是一个段落。

    这是一个段落。
    这不是一个段落。  
    （上行末尾有两个以上空格）这也是一个段落。
</details>

# 字体

<details>
<summary></summary>
*斜体* _斜体_

    *斜体* _斜体_

**粗体** __粗体__

    **粗体** __粗体__

***粗斜体*** ___粗斜体___

    ***粗斜体*** ___粗斜体___

~~删除线~~

    ~~删除线~~

<u>下划线</u>

    <u>下划线</u>

==高亮==（部分编辑器支持）

    ==高亮==（部分编辑器支持）
</details>

# 列表

<details>
<summary></summary>

## 无序列表



<details>
<summary></summary>

- 第一项
- 第二项  
- 第三项
    - 第三项子项

---

    - 第一项
    - 第二项  
    - 第三项
      - 第三项子项

* 第一项
* 第二项
* 第三项

---

    * 第一项
    * 第二项
    * 第三项

+ 第一项
+ 第二项
+ 第三项

---

    + 第一项
    + 第二项
    + 第三项

</details>


## 有序列表

<details>
<summary></summary>

1. 第一项
2. 第二项
3. 第三项
4. 第四项

---

    1. 第一项
    2. 第二项
    3. 第三项
    1. 第四项

    列表标记的数字不会影响渲染结果。下面的代码会渲染成与上面相同的有序列表。

---

1. 第一项
    - 子项A
    - 子项B
2. 第二项
    - 子项C
        - 子子项C1
        - 子子项C2

---

    1. 第一项
       - 子项A
       - 子项B
    2. 第二项
       - 子项C
         - 子子项C1
         - 子子项C2

1. 第一项
非段落
1. 第二项
    第二段的补充说明段落。

---

    1. 第一项
    非段落
    1. 第二项
        第二段的补充说明段落。
    因此列表的高亮需tab两次。

</details>
</details>


# 引用

<details>
<summary></summary>

> 这是一段引用文本
>> 第二层引用
>>>第三层引用

    > 这是一段引用文本
    >> 第二层引用
    >>>第三层引用

> #### 引用中的标题
> 1. 有序列表项一
> 2. 有序列表项二
> 这里是**粗体**和*斜体*示例

    > #### 引用中的标题
    > 1. 有序列表项一
    > 2. 有序列表项二
    > 这里是**粗体**和*斜体*示例

</details>

# 代码块

<details>
<summary></summary>

这是行内代码示例：`print("Hello Lantern")`

    这是行内代码示例：`print("Hello Lantern")`

```python
def hello():
    print("Hello Lantern")

hello()
```

    ```python
    def hello():
        print("Hello Lantern")

    hello()
    (可为：Python、C/C++、HTML、Bash/Shell/SQL)

另一种创建代码块的方式是使用tab/四个空格。

    def hello()
        print("Hello Lantern")

</details>

# 链接

<details>
<summary></summary>

[GitHub](https://github.com)  
[百度搜索](https://www.baidu.com)

    [GitHub](https://github.com)  
    [百度搜索](https://www.baidu.com)

[GitHub](https://github.com "GitHub 官网")

    [GitHub](https://github.com "GitHub 官网")
    鼠标悬停在链接上会显示双引号里的字。

这是一个 [GitHub][1] 链接示例。

[1]: https://github.com "GitHub 官网"

    这是一个 [GitHub][1] 链接示例。

    [1]: https://github.com "GitHub 官网"
    如此，可以将链接定义在其他位置，使正文更整洁。

<https://github.com>  
<example@email.com>

    <https://github.com>  
    <example@email.com>

</details>

# 图片

<details>
<summary></summary>

![示例图片](https://example.com/image.png)

    ![示例图片](https://example.com/image.png)
    基本语法：![替代文本](图片URL)

![示例图片](https://example.com/image.png "图片标题")

    ![示例图片](https://example.com/image.png "图片标题")
    鼠标悬停在图片上会显示双引号里的字。

![示例图片][image-ref]

[image-ref]: https://example.com/image.png "图片标题"

    ![示例图片][image-ref]

    [image-ref]: https://example.com/image.png "图片标题"

<img src="https://example.com/image.png" width="300" height="200" alt="示例图片">

    <img src="https://example.com/image.png" width="300" height="200" alt="示例图片">
    Markdown 本身不支持直接调整图片尺寸，需要使用 HTML 标签

</details>

# 表格

<details>
<summary></summary>

| 姓名 | 年龄 | 职业 |
|------|------|------|
| 张三 | 25   | 工程师 |
| 李四 | 30   | 设计师 |
| 王五 | 28   | 产品经理 |

    | 姓名 | 年龄 | 职业 |
    |------|------|------|
    | 张三 | 25   | 工程师 |
    | 李四 | 30   | 设计师 |
    | 王五 | 28   | 产品经理 |
    Markdown 表格使用竖线 | 分隔列，使用连字符 - 创建表头与内容分隔线。

| 左对齐 | 居中对齐 | 右对齐 |
|:--------|:--------:|--------:|
| 内容 A  | 内容 B   | 内容 C  |
| 内容 D  | 内容 E   | 内容 F  |

    | 左对齐 | 居中对齐 | 右对齐 |
    |:--------|:--------:|--------:|
    | 内容 A  | 内容 B   | 内容 C  |
    | 内容 D  | 内容 E   | 内容 F  |

| 功能 | 语法 | 示例 |
|------|------|------|
| 粗体 | `**文本**` | **粗体** |
| 斜体 | `*文本*` | *斜体* |
| 链接 | `[文本](URL)` | [GitHub](https://github.com) |

    | 功能 | 语法 | 示例 |
    |------|------|------|
    | 粗体 | `**文本**` | **粗体** |
    | 斜体 | `*文本*` | *斜体* |
    | 链接 | `[文本](URL)` | [GitHub](https://github.com) |

</details>

# 任务列表

<details>
<summary></summary>

- [ ] 未完成任务
- [x] 已完成任务
- [ ] 另一个未完成任务

      - [ ] 未完成任务
      - [x] 已完成任务
      - [ ] 另一个未完成任务

- [x] 学习 Markdown 基础语法
    - [x] 标题
    - [x] 列表
    - [x] 链接和图片
- [ ] 实践练习
    - [ ] 写一篇博客
    - [ ] 整理笔记

          - [x] 学习 Markdown 基础语法
            - [x] 标题
            - [x] 列表
            - [x] 链接和图片
          - [ ] 实践练习
            - [ ] 写一篇博客
            - [ ] 整理笔记

</details>

# 脚注

<details>
<summary></summary>

这是一个脚注示例[^1]，用于添加额外说明。

[^1]: 这是脚注的内容，会显示在文档末尾。

    这是一个脚注示例[^1]，用于添加额外说明。

    [^1]: 这是脚注的内容，会显示在文档末尾。

Markdown 由 John Gruber 创建[^gruber]，并在 GitHub 等平台得到广泛应用[^github]。

[^gruber]: John Gruber 是一位作家和软件开发者，也是 Daring Fireball 博客的创始人。  
[^github]: GitHub 是全球最大的代码托管平台，全面支持 Markdown 语法。

    Markdown 由 John Gruber 创建[^gruber]，并在 GitHub 等平台得到广泛应用[^github]。

    [^gruber]: John Gruber 是一位作家和软件开发者，也是 Daring Fireball 博客的创始人。
    [^github]: GitHub 是全球最大的代码托管平台，全面支持 Markdown 语法。

</details>

# 转义符

<details>
<summary></summary>

\* 不是斜体 \*  
\# 不是标题  
\[ 不是链接

    \* 不是斜体 \*
    \# 不是标题
    \[ 不是链接

</details>

# 高级技巧

<details>
<summary></summary>

## 部分文本

<details>
<summary></summary>

<div style="color: red;">红色文本</div>

    <div style="color: red;">红色文本</div>

<span style="font-size: 20px;">大号文本</span>

    <span style="font-size: 20px;">大号文本</span>

<details>
<summary>点击展开</summary>
隐藏的内容
</details>

    <details>
    <summary>点击展开</summary>
    隐藏的内容
    </details>

</details>

## 公式

<details>
<summary></summary>

行内公式：$E = mc^2$

    行内公式：$E = mc^2$

块级公式：
$$
\int_{a}^{b} f(x) \, dx = F(b) - F(a)
$$

    块级公式：
    $$
    \int_{a}^{b} f(x) \, dx = F(b) - F(a)
    $$

</details>

## 图表

<details>
<summary></summary>

```mermaid
graph LR
    A[开始] --> B{判断}
    B -->|是| C[执行 A]
    B -->|否| D[执行 B]
    C --> E[结束]
    D --> E
```

    ```mermaid
    graph LR
        A[开始] --> B{判断}
        B -->|是| C[执行 A]
        B -->|否| D[执行 B]
        C --> E[结束]
        D --> E
    ```

</details>

## 目录

<details>
<summary></summary>

在文章开头输入：  
[TOC] 
或  
[[toc]]  
或  
{{ .TOC }}  
可生成目录

</details>

    以上高级功能需要特定的 Markdown 解析器或编辑器支持。在使用前请确认目标平台的兼容性。例如，数学公式在 Typora、VS Code（Markdown Preview Enhanced）中支持良好，但 GitHub 原生不支持。

</details>