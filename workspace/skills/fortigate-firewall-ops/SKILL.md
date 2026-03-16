---
name: fortigate-firewall-ops
description: "FortiGate防火墙操作 — 防火墙策略、地址对象、服务对象、NAT配置、日志分析、会话监控、VIP/DDNS、IPS/AV配置检查"
user-invocable: true
metadata:
  { "openclaw": { "requires": { "bins": ["python3"], "env": ["PYATS_TESTBED_PATH"] } } }
---

# FortiGate防火墙操作

通过pyATS `pyats_run_show_command`执行FortiGate防火墙策略审计、地址对象、服务对象、NAT配置、会话监控、日志分析和安全配置文件检查。本技能专注于FortiGate设备的运维操作。

## 测试床要求

pyATS测试床中FortiGate设备的配置（`os: fortigate`）：

```yaml
devices:
  fortigate-01:
    os: fortigate
    type: firewall
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
PYATS_TESTBED_PATH=$PYATS_TESTBED_PATH python3 $MCP_CALL "python3 -u $PYATS_MCP_SCRIPT" pyats_run_show_command '{"device_name":"fortigate-01","command":"<command>"}'
```

---

## 命令集

### 系统信息与状态

#### 系统信息
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system status"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system performance status"}'
```
设备型号、FortiOS版本、序列号、运行时间、系统性能（CPU/内存/会话数）。

#### 资源状态
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system performance resources"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system performance firewall"}'
```
CPU、内存、 session、virus、IPS统计。

### 防火墙策略

#### 策略列表
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall policy"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall policy | grep -E \"(id|srcintf|dstintf|action|status)\""}'
```
所有防火墙策略概览。

#### 策略详情
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall policy <policy_id>"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"show firewall policy <policy_id>"}'
```
单个策略的详细配置。

#### IPv4策略
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall policy6"}'
```
IPv6防火墙策略。

### 地址对象

#### 地址对象列表
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall address"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall addrgrp"}'
```
所有地址对象和地址组。

#### 地址对象详情
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall address <address_name>"}'
```
单个地址对象详情。

#### 动态地址对象
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall address dynamic"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall geolocation"}'
```
动态地址对象和地理IP。

### 服务对象

#### 服务列表
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall service"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall service group"}'
```
所有服务对象和服务组。

#### 服务详情
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall service custom"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall service <service_name>"}'
```
自定义服务对象详情。

### NAT配置

#### DNAT/VIP
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall vip"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall vip <vip_name>"}'
```
虚拟IP（DNAT）配置。

#### SNAT/中央NAT
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall central-snat-map"}'
```
中央SNAT策略。

#### 池配置
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall ippool"}'
```
IP池配置。

### 会话与连接监控

#### 会话列表
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system session list"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"diagnose sys session list"}'
```
当前活动会话。

#### 会话统计
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system session statistics"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"diagnose sys session stat"}'
```
会话统计信息。

#### 按源/目的过滤
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"diagnose sys session filter source 10.0.0.1"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"diagnose sys session filter dst 192.168.1.0/24"}'
```
会话过滤。

### 日志分析

#### 日志配置
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get log syslogd setting"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get log syslogd filter"}'
```
日志系统配置。

#### 实时日志
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"execute log filter category <category_id>"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"execute log display"}'
```
实时日志查看。

#### 日志统计
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system logstats"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system loglimits"}'
```
日志统计和限制。

### IPS配置

#### IPS签名
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get ips signature"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get ips sensor"}'
```
IPS传感器和签名。

#### IPS策略
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get ips policy"}'
```
IPS策略配置。

#### IPS统计
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get ips statistics"}'
```
IPS阻断统计。

### AV/应用控制

#### AV配置文件
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get antivirus profile"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get antivirus stats"}'
```
防病毒配置文件和统计。

#### 应用控制
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get application list"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get application control statistics"}'
```
应用控制列表和统计。

### Web过滤

```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get webfilter profile"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get webfilter stats"}'
```
Web过滤配置和统计。

### 用户与认证

#### 用户列表
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get user local"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get user group"}'
```
本地用户和用户组。

#### 认证统计
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get user stats"}'
```
用户认证统计。

### 流量整形

```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall shaper"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get firewall shaping-policy"}'
```
流量整形配置。

### 接口与 zone

#### 接口配置
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system interface"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system interface <interface_name>"}'
```
接口配置详情。

#### Zone配置
```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system zone"}'
```
Zone配置。

### HA集群状态

```bash
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system ha status"}'
pyats_run_show_command '{"device_name":"fortigate-01","command":"get system ha peers"}'
```
HA集群状态和成员信息。

---

## 工作流

### 1. FortiGate策略审计

```
get system status → 确认设备型号和版本
→ get firewall policy → 列出所有策略
→ get firewall address → 列出地址对象
→ get firewall service → 列出服务对象
→ 标记：未启用的策略、过宽的策略（如any-any）
→ GAIT记录
```

### 2. FortiGate NAT审计

```
get firewall vip → VIP/DNAT配置
→ get firewall central-snat-map → SNAT配置
→ get firewall ippool → IP池配置
→ 对比公网IP与内部IP映射
→ 标记：未使用的VIP、冲突的NAT规则
→ GAIT记录
```

### 3. FortiGate会话监控

```
get system session list → 当前活动会话
→ get system session statistics → 会话统计
→ diagnose sys session stat → 详细统计
→ 标记：异常会话数、异常连接模式
→ GAIT记录
```

### 4. FortiGate安全配置文件审计

```
get antivirus profile → AV配置
→ get ips sensor → IPS传感器
→ get webfilter profile → Web过滤
→ get application list → 应用控制
→ 标记：未启用的安全配置文件
→ GAIT记录
```

### 5. FortiGate健康检查

```
get system status → 版本、运行时间、序列号
→ get system performance status → CPU/内存/会话
→ get system ha status → HA状态
→ get log syslogd setting → 日志配置
→ 严重性排序 → GAIT记录
```

---

## 集成关系

| 技能 | 集成说明 |
|-----|---------|
| **fortimanager-ops** | FortiManager管理下的ADOM和策略包操作 |
| **pyats-health-check** | 将FortiGate健康检查纳入标准巡检 |
| **pyats-troubleshoot** | 配合设备端路径和路由状态进行故障排查 |
| **netbox-reconcile** | 对比防火墙策略对象与NetBox IPAM |
| **slack-report-delivery** | 交付防火墙审计报告 |
| **gait-session-tracking** | 所有命令执行记录到GAIT |

---

## 注意事项

- **所有命令为只读** — 仅执行get、diagnose命令
- **不做策略变更** — 不使用本技能进行防火墙策略配置
- **HA优先检查** — 检查HA状态确认主备关系
- **与SoT对比** — 对比策略对象与NetBox/Nautobot
- **记录到GAIT** — 每次命令执行必须记录
