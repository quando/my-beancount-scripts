my-beancount-scripts
=====================

Beancount 我的个人自用脚本，包含以下功能：

1. 账单导入
   - 支付宝（CSV）
   - 微信（CSV）
   - 中信银行信用卡（EML邮件）
2. 账单去重
   - 支付宝的账单**不包含**支付渠道，因此如果同时导入支付宝和银行账单，必然会重复。
   - 另外由于个人有手工记账的习惯，因此导入账单时也可能出现重复现象。
   - 脚本根据金额与时间判断两笔账单是否相同。
   - 自动为重复账单添加支付宝和微信的订单号及时间。
3. 价格抓取（基于bean-price）
   - 10jqka抓取同花顺数据
   - coinmarketcap抓取BTC数据

## 使用

### 账单导入

```bash
python import.py 微信支付账单\(20190802-20190902\).csv --out out.bean
```
其会自动识别文件类型，不需人工判断。
配置见``accounts.py``.

因为支付宝的账单非常弱智，因此支付宝余额、余额宝、花呗都被统一归结到``Assets:Company:Alipay:StupidAlipay``，这个账户没有任何价值，建议自行设置余额宝、花呗等的余额，并将该账户pad到0.00余额。

### 价格抓取

示例配置：

```beancount
2010-01-01 commodity F161725
  export: "F161725"
  price: "CNY:modules.price_sources.10jqka/161725"

2010-01-01 commodity BTC
  export: "BTC"
  price: "CNY:modules.price_sources.coinmarketcap/bitcoin--cny"
```

```bash
bean-price main.bean -d 2019-04-01
```

## 开源协议

The MIT License