# SNMP agent侧的设置

（SNMP的架构就分station与agent侧）

下面是华为Quidway S3700系列交换机上有关snmp的配置

```
<complex_building-1>display current-configuration | begin snmp
 snmp-agent
 snmp-agent local-engineid 000007DB7F0000010000496A
 snmp-agent sys-info contact Peng, 18626256113
 snmp-agent sys-info location Complex Building, 1st floor
 snmp-agent group v3 complex-building
 snmp-agent group v3 complex-building privacy
 snmp-agent usm-user v3 unisko complex-building  authentication-mode md5 3!'R''J4+<E8<ADA=H%:6A!! privacy-mode des56 KACKR)O2_La2BR9%VO$UD1!!
```

注意这里使用的是带认证和加密的snmp v3 privacy模式，其中认证方式为`md5`，加密方式为`des`。可通过以下`net-snmp`命令, 在终端进行测试：

```
snmpwalk 192.168.101.2 -v 3 -a MD5 -l authPriv -X privPassword -x DES -u unisko -A authPassword
```

> 注意：其中的认证口令与加密口令，应采用大小写字母、数字形式，最少8位（经Cacti 1.0.1中测试过）。

而在Cacti中，于添加设备页面，选择SNMP v3 及相应认证和加密方式，并填入两个口令即可（但测试发现偶尔会爆出`SNMP error`错误）。
