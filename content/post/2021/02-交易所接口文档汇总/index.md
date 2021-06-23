---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "交易所接口文档汇总"
subtitle: ""
summary: ""
authors: [向天龙]
tags: [hkex,港交所,中华通,上交所,深交所,纳斯达克,接口文档]
categories: [金融知识库]
date: 2021-01-27T14:35:21+08:00
lastmod: 2021-01-28T20:53:21+08:00
featured: false
draft: false
toc: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## 前言

金融软件开发的第五个年头，打交道最多的就是各种交易所的接口文档，熟悉的也是港交所的文档，近期处理中华通业务，涉及到了部分中华通业务，顺手查阅了深交所和上交所的资料

## 港交所

[官网链接](https://www.hkex.com.hk/)

### 常用

* [交易時間，交易和結算日曆](https://www.hkex.com.hk/Mutual-Market/Stock-Connect/Reference-Materials/Trading-Hour,-Trading-and-Settlement-Calendar?sc_lang=zh-HK)
* [交易机制](https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/Services/Trading/Securities/Overview/Trading-Mechanism?sc_lang=zh-CN)
* [中港金融詞彙對照表](https://www.hkex.com.hk/-/media/hkex-market/mutual-market/stock-connect/reference-materials/resources/glossary_c)
---
* [滬港通及深港通交易日曆 PDF](2021-Calendar_pdf_c.pdf)
* [滬港通及深港通交易日曆 CSV](2021-Calendar_csv_c.csv)
* [中港金融詞彙對照表 PDF](glossary_c.pdf)
---
---
* [市调机制冷静期触发记录](https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/chi/vcm/vcmtriggersecurity_c.aspx)
* [证券名单：基本信息、证券分类](https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/chi/services/trading/securities/securitieslists/ListOfSecurities_c.xlsx)
* [收市竞价交易时段证券](https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/Services/Trading/Securities/Securities-Lists/Closing-Auction-Session-(CAS)-Securities?sc_lang=zh-CN)
* [市场波动调节机制（市调机制）证券](https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/Services/Trading/Securities/Securities-Lists/Volatility-Control-Mechanism-(VCM)-Securities?sc_lang=zh-CN)
* [可进行卖空的指定证券](https://sc.hkex.com.hk/TuniS/www.hkex.com.hk/Services/Trading/Securities/Securities-Lists/Designated-Securities-Eligible-for-Short-Selling?sc_lang=zh-CN)
---
* [证券名单：基本信息、证券分类 XLSX](ListOfSecurities_c.xlsx)
### 行情接口文档：港股 + 中华通

[行情接口文档汇总链接](https://www.hkex.com.hk/Services/Market-Data-Services/Infrastructure/Overview?sc_lang=en)

常见问题答疑、指引开发手册、历史行情接口文档可以通过搜索栏获取下载地址，搜索历史版本号

* [港股证券行情接口文档](https://www.hkex.com.hk/Services/Market-Data-Services/Infrastructure/HKEX-Orion-Market-Data-Platform-Securities-Market-OMD-C?sc_lang=en)
* [中华通行情接口文档](https://www.hkex.com.hk/Mutual-Market/Stock-Connect/Reference-Materials/Technical-Documents/OMD_CC-Specifications?sc_lang=en)
---
* [HKEX_OMDC_Binary_Interface_Specifications_v_1,-d-,32c.pdf](HKEX_OMDC_Binary_Interface_Specifications_v_1,-d-,32c.pdf)
* [HKEX_OMDC_Developers_Guide_1_11.pdf](HKEX_OMDC_Developers_Guide_1_11.pdf)
* [OMDC_Connectivity_Guide_Securities_Market-_Index_datafeed_(v2_2).pdf](OMDC_Connectivity_Guide_Securities_Market-_Index_datafeed_(v2_2).pdf)
* [OMD_Interface_Specification_China_Connect_Securities-(v1-3).pdf](OMD_Interface_Specification_China_Connect_Securities-(v1-3).pdf)
* [OMD_Connectivity_Guide_China_Connect_Securities.pdf](OMD_Connectivity_Guide_China_Connect_Securities.pdf)
* [OMD_Developers_Guide_China_Connect_Securities.pdf](OMD_Developers_Guide_China_Connect_Securities.pdf)

### 报盘接口文档：港股 + 中华通

[报盘接口文档汇总链接](https://www.hkex.com.hk/Services/Trading/Securities/Infrastructure/Overview?sc_lang=en)

* [港股FIX协议接口文档 PDF](HKEX_OCG_FIX_Trading_Interface_Specifications_v2_2-(clean).pdf)
* [港股二级制协议接口文档 PDF](HKEX_OCG_Binary_Trading_Interface_Specifications_v2_2-(clean).pdf)
* [港股交易所错误代码清单 XLSX](Reason_Text_List.xlsx)
---
* [中华通Fix协议接口文档 PDF](HKEx_CCCG_FIX_Trading_Interface_Specifications_v1_3-(clean).pdf)
* [中华通二进制接口文档 PDF](HKEx_CCCG_Binary_Trading_Interface_Specifications_v1_3-(clean).pdf)

## 上交所

[行情报盘接口文档](http://www.sse.com.cn/services/tradingservice/tradingtech/technical/data/)

报错接口文档在其他菜单中获取

[报盘报错接口文档 XLSX](SSE_IS111_ErrorCode_CV3.15.xlsx)

## 深交所

[行情报盘接口文档](http://www.szse.cn/marketServices/technicalservice/interface/)

深交所没有提供单独的报错信息说明，在报盘接口文档的第六章有附加说明

[深圳证券交易所Binary交易数据接口规范（Ver1.18）PDF](P020201229686784934466.pdf)

## 纳斯达克

* [假期安排](https://www.nyse.com/markets/hours-calendars)
* [新股上市信息](https://www.nasdaq.com/market-activity/ipos?tab=upcoming)

## 收市价

[全球市场收市价](http://eoddata.com/stocklist/NASDAQ.htm)