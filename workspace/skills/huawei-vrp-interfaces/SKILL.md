---
name: huawei-vrp-interfaces
description: "华为VRP接口操作 — 接口状态、流量统计、错误计数、聚合链路、LLDP邻居、光模块诊断、VLAN信息"
user-invocable: true
metadata:
  { "openclaw": { "requires": { "bins": ["python3"], "env": ["PYATS_TESTBED_PATH"] } } }
---

# 华为VRP接口操作

通过pyATS `pyats_run_show_command`执行华为VRP设备接口状态检查、流量统计、错误计数分析、链路聚合（Eth-Trunk）、LLDP邻居发现、光模块诊断和VLAN信息查询。

## 测试床要求

pyATS测试床中华为设备的配置（`os: huawei`）：

```yaml
devices:
  huawei-core-01:
    os: huawei
    type: router
    connections:
      cli:
        protocol: ssh
        ip: 10.0.0.1
        port: 22
    credentials:
      default:
        username: "%ENV{NETCLAW_USERNAME}"
        password: "%ENV{NETCLAW_PASSWORD}"
```

## 如何调用

```bash
PYATS_TESTBED_PATH=$PYATS_TESTBED_PATH python3 $MCP_CALL "python3 -u $PYATS_MCP_SCRIPT" pyats_run_show_command '{"device_name":"huawei-core-01","command":"<command>"}'
```

---

## 命令集

### 接口状态概览

#### 所有接口摘要
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display ip interface brief"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display interface brief"}'
```
快速查看所有接口的Admin/Protocol状态、IP地址。

#### 全部接口详情
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display interface"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display interface description"}'
```
所有接口的详细统计信息、描述信息。

### 特定接口详情

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display interface GigabitEthernet0/0/1"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display this interface"}'
```
单个接口的详细参数、计数器和状态。

### 流量统计

#### 接口统计
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display interface GigabitEthernet0/0/1"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display counters interface GigabitEthernet0/0/1"}'
```
接口的输入/输出字节、包速率、错误计数。

#### 流量趋势
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display traffic statistics interface GigabitEthernet0/0/1"}'
```
接口流量统计历史。

### 错误计数器

#### 接口错误计数
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display interface GigabitEthernet0/0/1"}'
```
检查以下错误计数器：
- Input errors (CRC错误、帧错误)
- Output errors
- Input drops
- Output drops
- Collisions
- Late collisions

**告警阈值：**
- CRC错误 > 0 → 警告：物理层问题（光纤/网线、光模块、双工不匹配）
- Input errors 增长 → 警告：数据包损坏
- Output drops > 0 → 警告：拥塞或QoS问题
- Resets 增长 → 严重：接口震荡

### 链路聚合（Eth-Trunk）

#### 聚合链路状态
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display eth-trunk"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display eth-trunk 1"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display eth-trunk 1 verbose"}'
```
Eth-Trunk成员状态、LACP协议状态。

#### 成员接口状态
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display trunkmember interface GigabitEthernet0/0/1"}'
```
特定成员接口的Trunk状态。

#### LACP统计
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display lacp statistics eth-trunk 1"}'
```
LACP协议PDU发送/接收计数。

### LLDP邻居

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display lldp neighbor"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display lldp neighbor brief"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display lldp neighbor interface GigabitEthernet0/0/1"}'
```
LLDP邻居信息：远程系统名、端口ID、 chassis ID。

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display lldp local"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display lldp interface"}'
```
本地LLDP配置和启用接口。

### 光模块诊断

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display transceiver interface GigabitEthernet0/0/1"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display transceiver diagnosis interface GigabitEthernet0/0/1"}'
```
光模块信息：
- 光模块型号、波长、传输距离
- 发射功率 (TX dBm)
- 接收功率 (RX dBm)
- 激光偏置电流
- 模块温度、电压

**告警阈值：**
- RX功率 < -20 dBm → 警告
- RX功率 < -25 dBm → 严重：光纤或光模块问题
- TX功率异常 → 警告

### VLAN信息

#### VLAN概览
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display vlan"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display vlan brief"}'
```
VLAN ID、名称、成员接口。

#### VLAN详情
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display vlan 10"}'
```
特定VLAN的详细信息。

#### 接口VLAN配置
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display port vlan interface GigabitEthernet0/0/1"}'
```
接口的VLAN配置。

### MAC地址表

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display mac-address"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display mac-address dynamic"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display mac-address interface GigabitEthernet0/0/1"}'
```
MAC地址表信息。

### ARP表

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display arp"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display arp interface Vlanif10"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display arp all"}'
```
ARP缓存表信息。

### BFD会话

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display bfd session"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display bfd session all"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display bfd session 10.1.1.2"}'
```
BFD会话状态，用于快速故障检测。

---

## 工作流

### 1. 华为VRP接口健康检查

```
display interface brief → 查找down/down接口
→ display interface {down_intf} → 错误计数、丢包
→ display transceiver diagnosis {intf} → 光模块功率
→ display lldp neighbor → 验证预期邻居
→ 严重性排序（down=严重，errors=警告）→ GAIT记录
```

### 2. 链路聚合审计

```
display eth-trunk → 查找聚合链路
→ display eth-trunk {trunk_id} verbose → 成员状态
→ display lacp statistics eth-trunk {trunk_id} → LACP统计
→ 标记：缺失成员、LACP不匹配、PDU错误
→ GAIT记录
```

### 3. 光模块批量扫描

```
pyats_list_devices → 查找华为设备（并行）
→ display transceiver diagnosis per device
→ 标记：RX功率 < -20 dBm (警告), < -25 dBm (严重)
→ 对比光模块序列号与NetBox
→ GAIT记录
```

### 4. 接口流量基线

```
display interface brief → 接口状态概览
→ display interface {intf} → 流量统计
→ display counters interface {intf} → 详细计数
→ 记录输入/输出速率、错误计数
→ GAIT记录
```

---

## 集成关系

| 技能 | 集成说明 |
|-----|---------|
| **huawei-vrp-system** | 系统/硬件上下文用于接口问题排查 |
| **huawei-vrp-routing** | 路由协议状态补充接口状态分析 |
| **pyats-topology** | LLDP数据用于拓扑发现 |
| **netbox-reconcile** | 对比接口IP、MAC、光模块序列号与NetBox |
| **pyats-parallel-ops** | pCall模式用于全网接口巡检 |
| **gait-session-tracking** | 所有命令执行记录到GAIT |

---

## 注意事项

- **所有命令为只读** — 仅执行show、display命令
- **不做接口配置变更** — 不使用本技能进行接口配置
- **对比光模块参数** — 始终对比诊断值与厂商规格
- **记录到GAIT** — 每次命令执行必须记录
- **检查错误计数** — 错误计数增长需关注
