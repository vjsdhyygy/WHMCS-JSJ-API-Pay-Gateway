# WHMCS-JSJ-API-Pay-Gateway

为金沙江支付免签API写的WHMCS接口，难得的好接口。请合理使用。。

本接口用于WHMCS与金沙江API接口对接，以完成支付宝/微信免签支付。

测试可用版本：WHMCS 6.x

理论支持5和7，未测试。

原作者不对使用该接口而产生的问题负责，请了解。

如果有需要手动补单的，可以在WHMCS系统中使用"添加付款"的功能，请注意填写交易流水号为：JSJApiPay_开头的唯一流水号，以防止重复回调被刷余额。

【PS：这个接口**不是帮你挂个监控手动转账订单**，而是由金沙江的即时到账接口代收货款，可以申请提现至支付宝或微信。】

## 快速开始

首先在金沙江API后台 [注册一个账户](//api.jsjapp.com/plugin.php?id=add:user&apiid=12744&from=github)。

下载本接口，并把文件放到站点的/modules/gateways/里。

进入WHMCS后台，系统设置-付款-支付网关-All Payment Gateways选项卡设置中：启用本接口（不同支付方式有独立配置），并在 Manage Existing Gateways 选项卡中填写APIID&APIKEY等，手续费仅用于WHMCS内部记账统计，但是必须填写（可以填0，WHMCS记账有手续费这么个特性，不会对实际支付金额产生影响）。

完成√

## 说明

代码仅供参考，有问题/BUG等请发Issue（请先确认您的WHMCS版本为6）。

使用的API为金沙江API：http://api.jsjapp.com/

依据金沙江官方PHP通用接口写的。未来官方有改动也会更新。

请了解这边订单号暂时是用UID做的，如果您的WHMCS系统订单数量超过99999999个，请联系我帮您修改为其他参数|´・ω・)ノ

PS：如果你需要吧手续费分摊给用户，请参考这个插件：[WHMCS-Gateway-Fees](https://github.com/delta360/WHMCS-Gateway-Fees) （第三方），本插件不会额外添加手续费（建议使用插件在之前添加，不建议在网关支付时添加，会产生取整影响回调判断的问题。）

（本来为自用，感觉金沙江提供的服务还是很不错的，所以公开了，请合理使用）

## TODO

为支付宝WAP方式等优化落地页。

## 更新日志

最近代码更新:2017-06-06 19:33

最近版本更新:v0.14-2017-06-06-01

2017-06-06-01：

改版，不同支付方式可独立设定。

新增微信扫码支付，并极致优化支付体验。

新增QQ钱包支付，支持手机扫码，浏览器唤醒QQ支付等，手机访问可直接启动。

回调地址等依据站点设置自动生成，无需手动修改了。

修正细节错误，简化设定。

逻辑优化，提早判断是否已抵达账单页面，兼容手续费插件，减少账单金额更改几率。

优化金额更改的错误提示（引导联系客服或自助充值）。

更多信息见https://github.com/tutugreen/WHMCS-JSJ-API-Pay-Gateway/commits/master

**请注意！文件名有更改！更新请删除所有旧文件！**

此版本已经过N遍测试优化可用，如遇问题优先发Issues。

2017-06-04-01：

Bugs Fix,修复callback内的拼写错误导致WAP无法正常回调的问题。

2017-06-04-01：

Bugs Fix,一些修正.

修复回调地址组合缺失系统地址的问题，修正拼写错误，修改名称，修改回调认证失败的目的地(WAP方式回调落地页待完善).

终于码完了微信扫码支付，极致优化支付体验。

2017-06-03-03：

独立出WAP端支付设定，适合纯手机WAP支付。电脑打开也是WAP支付，独立出来后需要至后台支付接口启用WAP支持和设定APIID等信息（可与WEB方式不同，但不建议）。

2017-06-03-02：

为支持多方式支付进行预改版，更改部分文件路径，优化部分代码，请需要更新的用户删除全部旧文件再安装！并需要重新至后台设定接口APIID等信息。

**目前如无特殊需求不需要手动修改回调地址了，系统会依据站点地址自动生成。**

**请注意如果您要申请微信支付等需要绑定回调地址的接口，请确认申请域名填写正确**

2017-06-01：

修正一些注释错误

2016-11-18：

修复回调非子目录，导致404的问题

2017-04-17：

更新官方新域名，并支持HTTPS。

请注意！官方已修改接口域名，请在2017-04-17之前下载接口的用户尽快更新代码（对支付和之前的订单无任何影响）

如果在2017年04月17日0点整仍未更改的，接口将无法使用。

**官方现已支持 HTTPS 回调地址**

2016-11-03:

依据官方要求更新参数编号规范

请注意！官方已修改订单编号(addnum)参数编号规范，请在2016-11-03之前下载接口的用户尽快更新代码（对支付和之前的订单无任何影响）

如果在2016年11月10日0点整仍未更改的，接口将无法使用。(详情见：[官方贴](//api.jsjapp.com/forum.php?mod=viewthread&tid=52))

## 常见问题

Q：订单流水号存在"JSJApiPay"，是否可以修改？

A：可以的，修改 callback/JSJApiPay_callback.php 相应部分即可(已注释指出)，请注意保持唯一性以防刷单。

Q：如何修改用户支付完后跳转到的落地页面。

A：编辑 callback/JSJApiPay_callback.php 相应部分即可(已注释指出)，可修改完成支付验证后的地址，默认为跳转到账单页面。

Q：部分支付方式需要审核（例如微信支付/QQ支付），如何开通？

A：登陆金沙江API，账户管理下可以开通相应接口，请注意审核的回调域名填写正确，以免无法正确回调。（截止2017-06-05，微信支付等还未正式开放，敬请期待！）

Q：为什么有时支付后系统已经到账，但实际账单未支付？

A：请检查订单金额是否和初次提交有更改，例如滞纳金等设定可能影响账单金额导致没有完全支付。

Q：为什么一开始能获取微信支付二维码的，但后来又获取失败了？

A：同上，当账单金额被更改时（例如客户手动使用余额支付部分款项，或者产生了滞纳金），获取会失败，遇到这些情况可以分拆账单，如有顾客有多笔未支付账单，可以使用合并支付生成新账单，或者自助充值，用余额支付剩余款项。

## 非本接口问题请联系

金沙江问题请去[官方板块](//api.jsjapp.com/forum.php?mod=forumdisplay&fid=36)咨询

窝的Email：yuanming@tutugreen.com

## Copyright and license

Copyright 2016~2017 Tutugreen.com. Code released under [the MIT license](https://github.com/tutugreen/WHMCS-JSJ-API-Pay-Gateway/blob/master/LICENSE).