# SNMP agent侧的设置

（SNMP的架构就分station与agent侧）

下面是华为Quidway S3700系列交换机上有关snmp的配置

```
<complex_building-1>display current-configuration | begin snmp
 snmp-agent
 snmp-agent local-engineid 000007DB7F0000010000496A
 snmp-agent sys-info contact Peng, 18626256113
 snmp-agent sys-info location Complex Building, 1st floor
 snmp-agent group v3 complex-building privacy
 snmp-agent usm-user v3 unisko complex-building  authentication-mode md5 3!'R''J4+<E8<ADA=H%:6A!! privacy-mode des56 KACKR)O2_La2BR9%VO$UD1!!
```

思科交换机上的SNMP v3配置，则更为简单，参考[AES and 3-DES Encryption Support for SNMP Version 3](http://www.cisco.com/c/en/us/td/docs/ios/12_4t/12_4t2/snmpv3ae.html#wp1053804)。

注意这里使用的是带认证和加密的snmp v3 privacy模式，其中认证方式为`md5`，加密方式为`des`。可通过以下`net-snmp`命令, 在终端进行测试：

此外，需要保持管理站与对象交换机（agent）IP地址的联通性（通常要将对象交换机的上行trunk接口设置为允许所有vlan通过`port trunk allow-pass vlan all`）。

```
snmpwalk 192.168.101.2 -v 3 -a MD5 -l authPriv -X privPassword -x DES -u unisko -A authPassword
```

> 注意：其中的认证口令与加密口令，应采用大小写字母、数字形式，最少8位（经Cacti 1.0.1中测试过）。

而在Cacti中，于添加设备页面，选择SNMP v3 及相应认证和加密方式，并填入两个口令即可（但测试发现偶尔会爆出`SNMP error`错误）。
