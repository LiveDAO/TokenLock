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

## 公众验证方法
多重签名账户的官方资料请参照下方链接
- [EOS.IO Technical White Paper v2](https://github.com/EOSIO/Documentation/blob/master/TechnicalWhitePaper.md#evaluating-permissions "EOS.IO Technical White Paper v2")

公众可以到任意EOS主网浏览器上查看三个池账户，示例如下
