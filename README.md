//打开QQ聊天界面就能自动聊天了
"auto"
t = 0; //延迟
toast("打开QQ消息界面自动聊天");
threads.start(notificationmsg);
while (true) {
  other = "";
  findMessage();
  othersay = getMessage();
  if (othersay != null) {
    var info = othersay;
    log("\n\nTa:\n" + ":" + othersay);
    i = "";
    i += Me();
    i += "";
    sendMessage(i);
    log("我:\n" + i);
    sleep(t)
    back();
  }
  findboxmsg();
}

function Me() {
  var url = "http://www.tuling123.com/openapi/api";
  id1 = "llmjk";
  var res = http.post(url, {
    key: "65458a5df537443b89b31f1c03202a80", //图灵
    //key: "e0fcda33be9847c8b8534abe51b4b095",  //唐嫣
    //  key: "b4438ca194064134afa28f686fbd04a5",  //my图灵
    info: info,
    userid: "1",
  });
  var html = res.body.string();
  eval("b=" + html);
  return b.text;
}

function getMessage() {
  var information = null;
  if (id("chat_item_head_icon").exists() && id("chat_item_content_layout").exists()) {

    var list = id("listView1").findOne();
    num = list.childCount();
    var n = 1;
    if (id("graybar").exists()) {
      i = 0;
      while (id("graybar").findOnce(i++) != null);
      n = i;
    }
    icon = id("chat_item_head_icon").findOnce(num - n);
    toastt = id("chat_item_content_layout").findOnce(num - n);
    if (toastt != null && icon != null) {
      x = icon.bounds().centerX();
      y = icon.bounds().centerY();
      if (x < 500) {
        information = toastt.text();
        other = icon.text();
      }
    }
  }
  return information;
}

function sendMessage(i) {
  if (id("input").depth("2").exists()) {
    id("input").depth("2").findOne().setText(i);
    className("android.widget.Button").text("发送").findOne().click();
  }
}

function findMessage() {
  if (id("unreadmsg").exists()) {
    unreadmsg = id("unreadmsg").findOnce(0);
    x1 = unreadmsg.bounds().centerX();
    y1 = unreadmsg.bounds().centerY();
    click(x1 - 200, y1 - 50);
    sleep(200);
  }
}

function findboxmsg() {
  if (id("msgbox").exists()) {
    id("msgbox").findOne().click();
  }
}

function notificationmsg() {
  var notifi;
  events.observeNotification();
  events.on("notification", function(n) {
    if ("com.tencent.mobileqq" == n.getPackageName()) {
      app.launch("com.tencent.mobileqq");
    }
  });
}
