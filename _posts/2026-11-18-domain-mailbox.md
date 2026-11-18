---
layout: post
title: 零成本搭一个自己的域名邮箱（实录）
---

一直想要一个自己的域名邮箱：注册网站时每个站留一个不同的地址，哪个开始发垃圾信就知道是谁漏的。今天花了两个多小时，把它搭出来了，全程 0 成本。记录一下过程，给想折腾的人参考。

### 用到的东西（全部免费）

- 一个免费子域名：zn626.ccwu.cc（在 DNSHE 领的，有效期约 10 年）
- Cloudflare 免费账号：管 DNS、托管邮箱程序、负责收信
- Cloud Mail：开源邮箱程序（GitHub 上的 maillab/cloud-mail），界面是完整的网页邮箱
- GitHub 账号：用它的一键部署把程序装到 Cloudflare 上

### 大致流程

1. 在 Cloudflare 注册账号，把域名加进去，选免费版
2. 回域名商（DNSHE）把域名的 NS 改成 Cloudflare 分配的两个地址，等它激活
3. 在 GitHub 上 fork Cloud Mail 项目，在 Cloudflare 建一个 API 令牌，把令牌和几个参数（域名、管理员邮箱、后台地址）填进仓库的 Secrets
4. 点 Run workflow，等它自动部署完
5. 在 Cloudflare 打开 Email Routing，把 Catch-all（全域名接收）指向刚部署的邮箱程序
6. 打开邮箱后台 mail.zn626.ccwu.cc，注册管理员账号 admin@zn626.ccwu.cc
7. 从 Gmail 发一封测试信——到了，全线打通

### 踩过的三个坑

- **令牌权限不够**：Cloudflare 的「Edit Cloudflare Workers」模板令牌不带数据库（D1）权限，部署会卡在建数据库那一步，要手动给令牌补上 D1 Edit 和 DNS Edit
- **workers.dev 没激活**：新账号要先打开一次 Workers 首页，账号的子域名才会生成，不然部署会报错
- **换 NS 后网站短暂打不开**：Cloudflare 给新域名签证书要十几分钟，这段时间原来的落地页是断的，别慌，等证书下来自己就好了

### 效果

- 任意前缀 @zn626.ccwu.cc 都能收信，等于无数个邮箱别名
- 后台是完整的网页邮箱，能建多个账号、分类管理
- 成本：0 元。Cloudflare、GitHub 都免费，域名是免费领的

### 一条提醒

这种免费子域名邮箱别用来绑银行卡和重要账号的找回邮箱——域名根不在自己手里。它最适合的场景就是：注册小号、收验证码、给每个网站留一个独立地址。
