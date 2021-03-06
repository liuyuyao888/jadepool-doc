
## 瑶池试用流程
欢迎试用瑶池，本文用于指导新用户试用瑶池，在试用前请确认已经收到试用许可邮件。邮件中包含服务器的地址、账号等信息，如果没有收到邮件或试用过程中有任何问题请联系我们，谢谢，试用愉快！


### 1 查看瑶池状态

- 请使用我们提供的账号和网址，登录瑶池admin后台管理网站:
![登录界面](https://ws2.sinaimg.cn/large/006tKfTcgy1g0u7316iqej31lo0u0n0m.jpg)
- 登录后首页:
![登录后首页](https://ws4.sinaimg.cn/large/006tKfTcgy1g0u7460da4j31of0u0jwr.jpg)
- 服务管理中，查看启动的链以及扫描区块状态:
![服务管理](https://ws3.sinaimg.cn/large/006tKfTcgy1g0u70z7bb7j31rm0u0tfe.jpg)

### 2 获取充值地址
- 执行指令

`curl -X POST http://127.0.0.1:7001/api/v1/addresses/new -H 'Accept: application/json' -H 'Content-Type:application/json' -d '{"data":{"type":"ETH","callback":"http://jadepool-callback:9008/callback"}}'`

详细说明:
```bash
curl -X POST \
  http://127.0.0.1:7001/api/v1/addresses/new \              // curl命令中的ip地址替换成瑶池服务器的IP，端口号和path部分不变
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"data":{"type":"ETH","callback":"http://jadepool-callback:9008/callback"}}'      // type: 指定币种
                                                                                        // callback: 回调地址
callback地址可以填自己的回调服务, 也可以用默认回调服务(http://jadepool-callback:9008/callback)；用默认的回调服务，可以通过http://IP:88/callback.txt 来查看callback的内容。
```

- 执行上述命令后，response：
```bash
{
	"code": 0,
	"status": 0,
	"message": "OK",
	"crypto": "ecc",
	"timestamp": 1551939008582,
	"sig": {
		"r": "qqZFTDKf6xvioCBrHsFE3DzhBL1czzq2Fuc5T2dT2ro=",
		"s": "FOoQy6jYDtQ4+BPSgReOBHNq7x7Zz1R5YsvaG0uIY3c=",
		"v": 27
	},
	"result": {
		"address": "0xc112dfed1222a806224e3b663bb489ff5a3be1b4",                               // 新生成的充值地址
		"type": "ETH",
		"state": "used",
		"namespace": "ETH",
		"sid": "APv5c1JVuzriaXOpAAAE"
	}
}
```
- 新生成的地址，在admin中可以查看到：
![查看地址](https://ws4.sinaimg.cn/large/006tKfTcgy1g0u7akzgfzj31pn0u044g.jpg)

### 3 充值
- 请线下用钱包，往新生成的充值地址中做一笔转账。
- 瑶池扫到这笔交易后，会生成一张充值订单，在admin中 订单管理->订单查询:
![查看充值订单](https://ws4.sinaimg.cn/large/006tKfTcgy1g0u80pggr6j31l40u07bn.jpg)
- 在钱包余额中查看，余额中是否增加这笔充值金额:
![钱包余额](https://ws1.sinaimg.cn/large/006tKfTcgy1g0u7xntufoj31jt0u0gs0.jpg)

- 查看充值回调：
![admin中查看订单回调](https://ws1.sinaimg.cn/large/006tKfTcgy1g0u85fa4s7j31oc0u0k4y.jpg)
如果用的默认回调服务，可以通过访问http://IP:88/callback.txt 来查看回调。

### 4 提现

- 执行以下指令：

`curl -X POST http://127.0.0.1:7001/api/v1/transactions/ -H 'Accept: application/json' -H 'Content-Type: application/json' -d '{"data":{"type":"ETH","to":"0x4dbd2672fc913f814e2d85fec20844ec0702d052","value":"0.01","extraData":"none"}}'`

详细说明:
```bash
curl -X POST \
  http://127.0.0.1:7001/api/v1/transactions/ \        // curl命令中的ip地址替换成瑶池服务器的IP，端口号和path部分不变
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"data":{"type":"ETH","to":"0x4dbd2672fc913f814e2d85fec20844ec0702d052","value":"0.01","extraData":"none"}}'     // type: 指定币种;to: 提现到的地址;  value: 提现金额;
```

- 上述提现指令执行后，response：
```bash
{
	"code": 0,
	"status": 0,
	"message": "OK",
	"crypto": "ecc",
	"timestamp": 1551941226168,
	"sig": {
		"r": "RrbTpSto4zqIZKCPlEwKuJQP9mILfSK/ScyjG/+8Fus=",
		"s": "JXwCYv5aYqN3/WQDV8PD4z7s+c/FguuTACTsGm6hekI=",
		"v": 27
	},
	"result": {
		"data": {},
		"id": "13",                                                               // 生成的提现单号
		"state": "init",
		"bizType": "WITHDRAW",
		"type": "ETH",
		"coinType": "ETH",
		"to": "0x65cA2bFcB3F9F5482e9947e5DDE4a66D9D997895",
		"value": "0.01",
		"confirmations": 0,
		"create_at": 1551941226160,
		"update_at": 1551941226162,
		"from": "0xb42edcca0fa411e972d172d7f5d18c59ea9084ad",
		"n": 0,
		"fee": "0",
		"fees": [],
		"hash": "",
		"block": -1,
		"extraData": "none",
		"memo": "",
		"sendAgain": false,
		"namespace": "ETH",
		"sid": "uMXsNwNT6-EpmDzyAAAB"
	}
}
```

- 瑶池生成一张提现订单:
![admin中查看提现订单](https://ws3.sinaimg.cn/large/006tKfTcgy1g0u8e8xseaj31jj0u0qes.jpg)
如果是用默认回调服务的话，可以通过访问http://IP:88/callback.txt, 查看回调

- 请确认收款地址是否收到这笔提现。