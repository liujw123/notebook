# 代理网易音乐

### 1. [安装node.js](./nvm_install.md)   

---

### 2. [安装项目](https://github.com/nondanee/UnblockNeteaseMusic)     

```

    https://github.com/nondanee/UnblockNeteaseMusic.git

```

---

### 3. 进入目录运行app.js测试

> 系统默认开一个8080端口，若8080端口还有其他用处，则可以加一个参数“-p port”来自定义端口。需要指定网易云服务器 IP -f xxx.xxx.xxx.xxx

```
    node app.js
```

---

### 4. 查ip

> 通过ping music.163.com获取IP 因此时dos已占用，所以在桌面上空白处按住Shift再右键 在弹出的菜单里选择“打开powershell窗口”

```
    ping music.163.com
```

---

### 5. 命令执行

> 8080”即端口 若自定义就输入自定义端口    “59.111.181.38”即之前查询的ip

```

    node app.js -p 8080 -f 59.111.181.38

```

---

### 6. 代理设置

> 打开网易云客户端 -> 设置 -> 工具 -> 代理 -> 测试 -> 重启

```

ip -> 127.0.0.7
端口 -> 8080/(自田)

```

--- 

### 7. 方便启动，创建bat执行文件

> 创建文件（unlock.txt） -> 复制命令 -> 另存为（ulock.bat）

```

    start cmd /k "cd /d D:\liujw155\study\UnblockNeteaseMusic&&node app.js -p 8080 -f 59.111.181.38"

    'D:\liujw155\study\UnblockNeteaseMusic' -> 项目地址

    'node app.js -p 8080 -f 59.111.181.38' -> 执行命令


```


>[原文](https://www.bilibili.com/read/cv3416428/)