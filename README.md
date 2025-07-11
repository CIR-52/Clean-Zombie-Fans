# 半自动清除B站僵尸粉

> 前段时间本人发现粉丝列表里有僵尸粉，所谓“僵尸粉”，就是指那些不参与互动的粉丝，对一个UP主来说，视频数据是最重要的，而僵尸粉会影响视频数据，所以需要清除。

> 在搜索清除僵尸粉时看到一篇专栏，专栏的作者使用浏览器控制台代码清除B站粉丝，但这样做缺点也非常大，会删除活跃粉。所以还是自己写了代码。

# 教程

在电脑上的浏览器打开[自己的个人主页](space.bilibili.com)。

如果是新版页面，请点击右下角的`返回旧版`

<img width="1175" height="619" alt="屏幕截图 2025-07-11 132134" src="https://github.com/user-attachments/assets/5552221a-b4a1-4b31-aba7-ae784ae267a5" />

点击`粉丝数`，按下键盘上的F12，或右键页面点击`检查`，打开 DevTools，点击`控制台`。

<img width="412" height="266" alt="屏幕截图 2025-07-11 132951" src="https://github.com/user-attachments/assets/973aeaaa-45c1-4b06-98ff-e1584fd91a64" />

在`>`的后面粘贴以下代码并回车：

`
// 引入jQuery库
var jq = document.createElement('script');
jq.src = "//code.jquery.com/jquery-3.6.0.min.js";
document.getElementsByTagName('head')[0].appendChild(jq);

var count = 0;

// 筛选需要移除的粉丝并点击移除按钮
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

// 处理确认移除粉丝的弹窗
function confirmRemoval() {
    $(".modal").each(function () {
        var $this = $(this);
        var modalTitle = $this.find(".modal-title").text();
        if (modalTitle === "确认移除粉丝") {
            console.log("正在删除粉丝: " + $this.find("em").text());
            $this.find(".btn-content:contains('确定')").click(); // 点击确认按钮
        }
    });
}

// 定时执行移除和确认操作
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

// 等待jQuery加载完成后再执行
jq.onload = timeout;
`

第一次会显示以下内容：

> 警告: 不要将代码粘贴到不了解或尚未审阅自己的 DevTools 控制台中。这可能导致攻击者窃取你的身份或控制你的计算机。请在下面键入“允许粘贴”，然后按 Enter 以允许粘贴。

此时输入`允许粘贴`并回车后再次粘贴代码回车。

此时代码会
