+++
title = "Week 071"
date = "2025-05-11T20:31:35+08:00"
description = "20250505 - 20250511"
tags = ["journal","reading"]
+++

## Reading Log

* 《反思的爱》

## Other

用Ruby写了一个简单的爬虫，用[Selenium](https://www.selenium.dev/)的无头浏览器获取[B.watch](https://b.watch/#/)上Box的价格和当前流通量。在自己的笔记本上成功运行后放在服务器上，开始出现问题：首先是找不到chromedriver，安装后解决；然后是网络超时，最后用clash for linux解决，期间还试过放到一个位于东京的服务器上，结果因为不在日本提供服务，只看到一个通知，后来想到应该可以通过点击按钮然后再加载页面。成功获取数据后，在crontab中增加一个定时任务。为了获取定时任务执行失败的信息，安装了postfix和mutt，用来接收和查看邮件。

用Python写了一个将分数分解为单位分数之和的小程序。

在小区看了另一个房子，布局不错，如果没什么大问题准备租下来。

本周比特币价格再次突破10万美元。
