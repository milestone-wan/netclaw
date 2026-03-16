---
name: huawei-vrp-system
description: "华为VRP系统操作 — 设备健康检查、硬件状态、版本信息、NTP、SNMP、系统日志、环境监控、内存CPU分析"
user-invocable: true
metadata:
  { "openclaw": { "requires": { "bins": ["python3"], "env": ["PYATS_TESTBED_PATH"] } } }
---

# 华为VRP系统操作

通过pyATS `pyats_run_show_command`执行华为VRP设备系统健康检查、硬件状态监控、版本信息查询、NTP/SNMP配置检查、系统日志分析、环境监控和资源利用分析。本技能为华为设备提供与JunOS/Cisco IOS系统操作类似的能力。

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

### 设备版本与基本信息

#### 版本信息
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display version"}'
```
显示设备型号、VRP版本、运行时间、硬件版本、序列号等基本信息。

#### 设备详细信息
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display device"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display device manufacture-info"}'
```
设备制造信息、序列号、生产日期等。

#### 运行时间
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display clock"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display time"}'
```
系统时钟和运行时间信息。

### CPU与内存监控

#### CPU使用率
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display cpu-usage"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display cpu-usage history"}'
```
CPU实时和历史使用率。**告警阈值：**
- 50%以下 → 正常
- 50-80% → 警告：CPU负载偏高
- 80-95% → 高：需要调查
- 95%以上 → 严重：需立即处理

#### 内存使用率
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display memory"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display memory usage"}'
```
内存使用情况统计。**告警阈值：**
- 70%以下 → 正常
- 70-85% → 警告：内存压力
- 85-95% → 高：可能影响路由表更新
- 95%以上 → 严重：进程可能崩溃

#### 进程状态
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display process cpu"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display process memory"}'
```
各进程CPU和内存占用详情。

### 硬件状态

#### 设备状态
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display device status"}'
```
所有板卡、模块的工作状态。

#### 风扇状态
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display fan"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display fan detail"}'
```
风扇转速、工作状态。

#### 电源状态
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display power"}'
```
电源模块输入/输出功率、状态。

#### 环境监控
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display environment"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display temperature"}'
```
设备温度、湿度等环境参数。

#### 单板信息
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display board"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display esn"}'
```
所有单板的ESN序列号和状态。

### NTP配置与状态

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display ntp status"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display ntp session"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display current-configuration | include ntp"}'
```
NTP同步状态和配置。**检查项：**
- NTP服务器可达性
- 时钟偏差（应<100ms）
- 同步状态（stratum级别）

### SNMP配置

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display snmp-agent statistics"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display snmp-agent community"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display snmp-agent usm-user"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display current-configuration | include snmp"}'
```
SNMP配置和统计信息。

### 系统日志

#### 日志缓冲
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display logbuffer"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display logbuffer level warning"}'
```
日志缓冲区内容，按级别过滤。

#### 告警信息
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display alarm"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display alarm all"}'
```
设备告警信息，包括硬件、软件告警。

#### 日志信息
```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display loginformation"}'
```
日志文件统计信息。

### 文件系统

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"dir"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display flash"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display startup"}'
```
文件系统使用情况、启动配置文件。

### 用户与登录

```bash
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display users"}'
pyats_run_show_command '{"device_name":"huawei-core-01","command":"display login"}'
```
当前登录用户、会话信息。

---

## 工作流

### 1. 华为VRP设备健康检查

```
display version → 设备型号、VRP版本、运行时间、序列号
→ display device status → 单板状态
→ display cpu-usage → CPU使用率
→ display memory → 内存使用率
→ display fan → 风扇状态
→ display power → 电源状态
→ display temperature → 温度监控
→ display ntp status → NTP同步状态
→ display alarm → 告警信息
→ 严重性排序 → GAIT记录
```

### 2. 华为VRP系统审计

```
display version → VRP版本、型号、序列号
→ display clock → 运行时间（<24小时需关注）
→ display memory usage → 内存使用率
→ display logbuffer level warning → 关键日志
→ display ntp status → 时间同步状态
→ 对比VRP版本与NVD CVE数据库
→ GAIT记录
```

### 3. 华为VRP硬件资产审计

```
display device → 设备信息
→ display board → 单板ESN
→ display esn → 序列号清单
→ display device manufacture-info → 制造信息
→ 对比NetBox DCIM资产
→ GAIT记录
```

---

## 集成关系

| 技能 | 集成说明 |
|-----|---------|
| **huawei-vrp-interfaces** | 接口操作补充系统/硬件视图 |
| **huawei-vrp-routing** | 路由协议补充系统/硬件视图 |
| **pyats-health-check** | 将华为设备健康检查纳入标准巡检 |
| **netbox-reconcile** | 对比硬件资产（序列号、型号）与NetBox DCIM |
| **nvd-cve** | 扫描VRP版本对应的CVE漏洞 |
| **gait-session-tracking** | 所有命令执行记录到GAIT |

---

## 注意事项

- **所有命令为只读** — 仅执行show、display命令
- **优先检查告警** — `display alarm`结果需优先处理
- **与SoT对比** — 对比硬件清单与NetBox/Nautobot
- **GAIT记录** — 每次命令执行必须记录
- **CPU/内存阈值** — 超过阈值需记录警告
