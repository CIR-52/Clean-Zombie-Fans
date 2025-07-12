# 注意事项

代码只能在旧版个人主页运行，因B站自身原因，只能清除前50页中的僵尸粉。

僵尸粉判定条件:

等级为Lv0的(未经测试)

或头像为默认小电视的(已测试)

或昵称以 bili_ 开头的(已测试)

# 教程

在电脑上的浏览器打开[自己的个人主页](space.bilibili.com)。

如果是新版页面，请点击右下角的`返回旧版`。

<img width="1175" height="619" alt="屏幕截图 2025-07-11 132134" src="https://github.com/user-attachments/assets/5552221a-b4a1-4b31-aba7-ae784ae267a5" />

点击`粉丝数`，按下键盘上的F12，或右键页面点击`检查`，打开 DevTools，点击`控制台`。

<img width="412" height="266" alt="屏幕截图 2025-07-11 132951" src="https://github.com/user-attachments/assets/973aeaaa-45c1-4b06-98ff-e1584fd91a64" />

在`>`的后面粘贴以下代码并回车：

```
var jq = document.createElement('script');
jq.src = "//code.jquery.com/jquery-3.6.0.min.js";
document.getElementsByTagName('head')[0].appendChild(jq);
var count = 0;
function removeFans() {
    $(".list-item").each(function () {
        var $this = $(this);
        var avatarSrc = $this.find(".bili-avatar-img").attr("src");
        var nickname = $this.find(".fans-name").text();
        var levelElement = $this.find(".m-level"); // 等级元素有 .m-level 类
        var isLevel0 = levelElement && levelElement.attr("lvl") === '0';
        var isDefaultAvatar = avatarSrc && avatarSrc.includes("noface.jpg");
        var isBiliNickname = nickname.startsWith("bili_");

        if (isLevel0 || isDefaultAvatar || isBiliNickname) {
            $this.find(".icon-ic_more").click(); // 点击更多操作按钮
            setTimeout(() => {
                $this.find(".be-dropdown-item:contains('移除粉丝')").click(); // 点击移除粉丝选项
            }, 500);
            return false;
        }
    });
}
function timeout() {
    setTimeout(function () {
        if (count % 2 === 0) {
            removeFans();
        } else {
            confirmRemoval();
        }
        count++;
        timeout();
    }, 2100);
}
jq.onload = timeout;
```

第一次会显示以下内容：

> 警告: 不要将代码粘贴到不了解或尚未审阅自己的 DevTools 控制台中。这可能导致攻击者窃取你的身份或控制你的计算机。请在下面键入“允许粘贴”，然后按 Enter 以允许粘贴。

此时输入`允许粘贴`并回车后再次粘贴代码回车。

按键盘上的`Ctrl`和`-`，页面会缩小，将页面调整到正好能显示一页的粉丝的大小。

代码后过一会儿会自动弹出僵尸粉的删除按钮，点击`确定`。删除后会刷新页面。

<img width="1386" height="1332" alt="屏幕截图 2025-07-10 162028" src="https://github.com/user-attachments/assets/ad4c1115-6c7a-445c-8efc-4f7901907a3c" />

当页面中没有僵尸粉时，请自己点击`下一页`，代码会自动查找下一页的僵尸粉。
