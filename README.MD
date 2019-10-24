# LIVE资产锁定介绍
## 初始LIVE分配
初始LIVE将分别储存在三个池账户中
- livedaogroup（600,000,000 LIVE）
- livedaofound（400,000,000 LIVE）
- livedaopool1（2,000,000,000 LIVE）

## 一阶段权限移交
由彼此独立的三方创建三个锁账户
- livedaolocka
- livedaolockb
- livedaolockc

处理livedaogroup账户：
1. 将此账户的owner和active权限阈值改为2
2. 移除owner和active权限中的单一key，更换为上面三个锁账户的active权限
3. 每个锁账户的权限权重设为1，此时这个池账户已经成为多重签名模式账户

处理livedaofound账户和livedaopool1账户（方式同上）

移交结果：必须拥有至少2个锁账户的active权限才能控制池账户的一切操作

## 二阶段权限完善
1. 如果有更多方加入，就创建各自相应的锁账户
2. 在超过阈值数量的的锁账户同意的情况下，即可更新池账户的权限，加入新的锁账户
3. 同时在所有锁账户的权重都是1的前提下，更改池账户一个合适的阈值

## 官方参考资料
多重签名账户的官方资料请参照下方链接
- [EOS.IO Technical White Paper v2](https://github.com/EOSIO/Documentation/blob/master/TechnicalWhitePaper.md#evaluating-permissions "EOS.IO Technical White Paper v2")

## 公众验证方法
1. 公众可以post数据到主网的任意可信任节点提供的api查看池账户权限
```
curl --request POST \
    --url https://eos.greymass.com/v1/chain/get_account \
    --header 'accept: application/json' \
    --header 'content-type: application/json' \
    --data '{"account_name":"livedaogroup"}';
```
得到数据
```
{
    "account_name": "livedaogroup",
    ...
    "permissions": [
        {
            "perm_name": "active",
            "parent": "owner",
            "required_auth": {
                "threshold": 2,
                "keys": [],
                "accounts": [
                    {
                        "permission": {
                            "actor": "livedaolocka",
                            "permission": "active"
                        },
                        "weight": 1
                    },
                    {
                        "permission": {
                            "actor": "livedaolockb",
                            "permission": "active"
                        },
                        "weight": 1
                    },
                    {
                        "permission": {
                            "actor": "livedaolockc",
                            "permission": "active"
                        },
                        "weight": 1
                    }
                ],
                "waits": []
            }
        },
        {
            "perm_name": "owner",
            "parent": "",
            "required_auth": {
                "threshold": 2,
                "keys": [],
                "accounts": [
                    {
                        "permission": {
                            "actor": "livedaolocka",
                            "permission": "active"
                        },
                        "weight": 1
                    },
                    {
                        "permission": {
                            "actor": "livedaolockb",
                            "permission": "active"
                        },
                        "weight": 1
                    },
                    {
                        "permission": {
                            "actor": "livedaolockc",
                            "permission": "active"
                        },
                        "weight": 1
                    }
                ],
                "waits": []
            }
        }
    ],
    ...
}
```
2. 公众可以到任意EOS主网浏览器上查看三个池账户
- [https://eospark.com/account/livedaogroup](https://eospark.com/account/livedaogroup "https://eospark.com/account/livedaogroup")

![](https://github.com/LiveDAO/TokenLock/blob/master/example.jpg?raw=true)

## 社区LIVE锁定
创建两个社区锁定账户（社区A和社区B）
- livecomlocka
- livecomlockb

从池账户livedaofound中通过多重签名转出LIVE到两个锁定账户，最终余额分配如下
- livedaofound（200,000,000 LIVE）
- livecomlocka（100,000,000 LIVE）
- livecomlockb（100,000,000 LIVE）

请查看EOS主网上这两笔交易的详情
- [https://eosflare.io/tx/25B8ECC092CF0B3FD6FBBAAF61DCD453F0D7ADFE800223C5818B06C7099993F3](https://eosflare.io/tx/25B8ECC092CF0B3FD6FBBAAF61DCD453F0D7ADFE800223C5818B06C7099993F3 "https://eosflare.io/tx/25B8ECC092CF0B3FD6FBBAAF61DCD453F0D7ADFE800223C5818B06C7099993F3")
- [https://eosflare.io/tx/0C6EF7C2D2913FD61EDBAE233CB63D51E915FAA7204D6010309D91B970F21A43](https://eosflare.io/tx/0C6EF7C2D2913FD61EDBAE233CB63D51E915FAA7204D6010309D91B970F21A43 "https://eosflare.io/tx/0C6EF7C2D2913FD61EDBAE233CB63D51E915FAA7204D6010309D91B970F21A43")

锁定说明（重要）
- livedaofound转给livecomlocka的交易中memo信息就是社区A锁定的LIVE对应的等额ERC20代币合约地址
- livedaofound转给livecomlockb的交易中memo信息就是社区B锁定的LIVE对应的等额ERC20代币合约地址
- 只有区块链上可查的这两个ERC20合约地址是官方认可的合法映射代币，其他同名ERC20代币统一视为山寨币
