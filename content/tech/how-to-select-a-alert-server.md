---
title: "如何选择一个消息提醒服务"
date: 2021-11-24T08:11:02+08:00
toc: false
images:
tags:
- 消息提醒
- alert
---

我一直挺关注并希望有一个消息推送工具，感觉有了这样一个东西仿佛能做很多东西（比如 TODO 到期发个通知，或者收到（不怎么看的）QQ 上重要的通知转发一下），今天把之前看到的各种工具以及自己的感受总结一下。

先说一下主要的几个推送手段

|推送手段			|优点		|缺点													|
|--	|--	|--	|
|微信服务号推送		|基于微信	|需要有服务号（目前只能企业/组织申请），用他人的则有限制|
|微信个人号			|基于微信	|需要购买 Wechaty Token									|
|企业微信应用推送	|基于微信	|企微推送藏在微信列表中（类似公众号通知），不够优雅		|
|企业微信消息推送	|基于微信	|企微推送藏在微信列表中（类似公众号通知），不够优雅		|
|短信推送			|无网可用	|贵，API 要 3 分一条									|
|邮件推送			|便宜且方便	|大部分人不会实时check邮箱，及时性差					|
|安卓推送			|便宜且方便	|iOS 不可用，且有概率被内存管理杀掉						|
|iOS 推送			|便宜且方便	|安卓不可用												|
|Telegram 推送		|便宜且方便	|大部分人没有一直开梯子查看 tg 消息的习惯				|
|飞书/钉钉推送		|便宜且方便	|大部分不会一直盯飞书/钉钉								|
|Webhook			|可自定义	|门槛高，需要一台服务器/会用云函数						|

我更倾向于 全部消息 -> 安卓/iOS，短信验证码+手机消息 -> 邮件推送。

现在介绍几个解决方案：

|名称			|网址										|支持手段										|优点									|缺点															|
|--	|--	|--	|--	|--	|
|饭碗			|https://fwalert.com/contacts				|邮件｜短信｜电话｜tg｜安卓						|支持安卓推送｜支持 webhook 的触发器	|邮件推送收费｜短信推送很贵｜安卓不稳定，开了守护仍然会被杀掉	|
|NextRT			|https://app.nextrt.com/user/info			|邮件｜短信｜电话｜tg｜企微｜飞书｜钉钉｜安卓	|支持安卓推送｜以GET/POST请求作为触发器	|没开源，无法私有化部署											|
|SmsForwarder	|https://github.com/pppscn/SmsForwarder		|邮件｜短信｜电话｜tg｜企微｜飞书｜钉钉｜Bark	|可以转发手机短信｜开源					|也只能转发手机短信｜安卓不稳定，概率会被杀掉					|
|Message-Pusher	|https://github.com/lyleshaw/Message-Pusher	|邮件｜企微｜Webhook							|开源｜容易自定义						|支持手段少														|

我目前的一个实现是：SmsForwarder 转发短信到邮件（方便电脑查看验证码），NextRT（安卓+企微）作为消息通知服务。

但是感觉目前的各种服务还是存在各种问题，且好用的安卓推送都没开源...

（希望我如果有时间学 flutter 的话能做一套开源的

（希望我有时间做一个 python-wechaty 的 alert plugin 作为微信个人号推送手段