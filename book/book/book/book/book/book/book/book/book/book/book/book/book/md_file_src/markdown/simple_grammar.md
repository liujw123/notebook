## markdown 基本语法   

**Markdown是一种纯文本格式的标记语言。通过简单的标记语法，它可以使普通文本内容具有一定的格式。**   

---

### 标题   

**一个`#`代表一级，上限六级**   

```
ex:
    # 一级标题
    ## 二级标题
    ### 三级标题
    #### 四级标题
    ##### 五级标题
    ###### 六级标题

```

##### 样例   

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

---

### 字体   

**加粗/斜体/斜粗/删除**   


```
ex:
    **加粗**
    *倾斜*
    ***斜体加粗***
    ~~删除线~~

```

##### 样例   

**加粗**   
*倾斜*   
***斜体加粗***   
~~删除线~~   

---

### 分割线   

**三个`*`/三个`_`/三个`-`**   

```
ex:
    ***
    ---
    ___

```


---

### 引用

**分层级引用，一个`>`代表一个层级**   

```
ex:
    > 一级引用
    >> 二级引用
    >>> 三级引用
    .....

```

##### 样例   

> 一级引用
>> 二级引用
>>> 三级引用

---

### 链接

```

    [链接名（必填）](地址（必填） "超链接title"（选填）)

ex：

    [链接](www.***.com)

```

##### 样例   

[链接](www.baidu.com)

---

### 图片

```

    ![加载失败显示](地址（必填） '停留显示')

ex:

    ![图片](abc.jpg 'title')

```
##### 样例   

![图片](../../assets/images/aaa.jpg '图片')

---

### 代码块   

```

    单行用一个反引号包围  (`)
    多行用三个反引号包围  (```)

    // 括号防转译，实际使用去'()'
    
```   

##### 样例  

`这是一行nb代码`

```
这是多行nb代码
这是多行nb代码
这是多行nb代码
```


--- 


### 列表   

**tab实现嵌套**   

#### 无序:（`*`/`+`/`-`） + 空格 + 内容   

```
ex:
    * 一层
        + 二层
            - 三层


```

##### 样例   

* 一层
    + 二层
        - 三层

#### 有序：数字 + `.` + 空格 + 内容    

```
ex:
    1. 一层
        1. 二层
        2. 二层
        3. 二层
    2. 一层
        1. 二层
        2. 二层
        3. 二层

```

##### 样例   

1. 一层
    1. 二层
    2. 二层
    3. 二层
2. 一层
    1. 二层
    2. 二层
    3. 二层

---


###表格 

```
ex：
    |123|345|567|
     |:-|:-:|-:|
    |qaz|wsx|edc|
    |qaz|wsx|edc|
    |qaz|wsx|edc|

-> 第一行是表头，对齐方式上只允许一行表头
-> 第二行是对齐方式，ex-> (|:-|)左对齐 / (|:-:|)居中 / (|-:|)右对齐
-> 第三行开始为表格内容
-> ....

```

##### 样例

|123|345|567|
 |:-|:-:|-:|
|qaz|wsx|edc|
|qaz|wsx|edc|
|qaz|wsx|edc|

