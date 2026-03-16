---
name: h3c-comware-routing
description: "H3C Comware路由操作 — OSPF、BGP、IS-IS路由协议分析、路由表查看、MPLS/ldp/RSVP状态、连通性测试"
user-invocable: true
metadata:
  { "openclaw": { "requires": { "bins": ["python3"], "env": ["PYATS_TESTBED_PATH"] } } }
---

# H3C Comware路由操作

通过pyATS `pyats_run_show_command`执行H3C Comware设备路由协议（OSPF、BGP、IS-IS）分析、路由表查看、MPLS/LDP/RSVP状态检查、连通性测试（ping、traceroute）。

## 测试床要求

pyATS测试床中H3C设备的配置（`os: h3c`）：

```yaml
devices:
  h3c-core-01:
    os: h3c
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
PYATS_TESTBED_PATH=$PYATS_TESTBED_PATH python3 $MCP_CALL "python3 -u $PYATS_MCP_SCRIPT" pyats_run_show_command '{"device_name":"h3c-core-01","command":"<command>"}'
```

---

## 命令集

### OSPF协议

#### OSPF邻居
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf peer"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf peer brief"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf peer interface GigabitEthernet1/0/1"}'
```
OSPF邻居状态，验证所有邻接关系是否为Full状态。

#### OSPF接口
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf interface"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf interface GigabitEthernet1/0/1"}'
```
OSPF接口状态、网络类型、Cost值、DR/BDR选举结果。

#### OSPF LSDB
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf lsdb"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf lsdb router"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf lsdb network"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf lsdb ase"}'
```
OSPF链路状态数据库内容。

#### OSPF路由
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf routing"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf routing 10.0.0.0/24"}'
```
OSPF学习到的路由。

#### OSPF统计
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf statistics"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf error"}'
```
OSPF协议统计信息、错误计数。

#### OSPF进程
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf brief"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ospf process"}'
```
OSPF进程概览、区域信息。

### BGP协议

#### BGP邻居摘要
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp peer"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp peer ipv4 unicast"}'
```
BGP邻居状态（Idle/Connect/Active/OpenSent/OpenConfirm/Established）。

#### BGP邻居详情
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp peer 10.1.1.2 verbose"}'
```
单个BGP邻居的详细信息：保持时间、AS号、路由表数量。

#### BGP路由表
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp routing-table"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp routing-table 10.0.0.0/24"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp routing-table as-path-acl"}'
```
BGP路由表条目、前缀数量。

#### BGP邻居路由
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp peer 10.1.1.2 received-routes"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp peer 10.1.1.2 advertised-routes"}'
```
从邻居接收/向邻居宣告的路由。

#### BGP统计
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp error"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp statistics"}'
```
BGP协议统计信息。

#### BGP组
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp group"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display bgp group group-name"}'
```
BGP组配置和状态。

### IS-IS协议

#### IS-IS邻居
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis peer"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis peer verbose"}'
```
IS-IS邻居状态。

#### IS-IS接口
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis interface"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis interface GigabitEthernet1/0/1"}'
```
IS-IS接口配置和状态。

#### IS-IS LSDB
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis lsdb"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis lsdb verbose"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis lsdb level-1"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis lsdb level-2"}'
```
IS-IS链路状态数据库。

#### IS-IS路由
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis route"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis route 10.0.0.0/24"}'
```
IS-IS学习到的路由。

#### IS-IS拓扑
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis topology"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis topology level-1"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display isis topology level-2"}'
```
IS-IS网络拓扑。

### 路由表

#### IPv4路由表
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table 10.0.0.0/24"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table statistics"}'
```
IPv4路由表和统计信息。

#### 按协议查看路由
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table protocol ospf"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table protocol bgp"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table protocol static"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table protocol direct"}'
```
按协议筛选路由。

#### 路由详情
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip routing-table 10.0.0.1 verbose"}'
```
单条路由的详细信息，包括下一跳、出接口等。

### MPLS

#### MPLS LDP
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display mpls ldp"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display mpls ldp session"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display mpls ldp peer"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display mpls ldp interface"}'
```
LDP会话和邻居状态。

#### MPLS LSP
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display mpls lsp"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display mpls lsp 10.0.0.0/24"}'
```
MPLS标签交换路径。

#### RSVP
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display rsvp session"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display rsvp neighbor"}'
```
RSVP会话和邻居状态。

#### MPLS接口
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display mpls interface"}'
```
MPLS使能接口状态。

### 连通性测试

#### Ping
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"ping 10.0.0.2"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"ping -c 5 10.0.0.2"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"ping -a 10.0.0.1 10.0.0.2"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"ping -s 1500 10.0.0.2"}'
```
基础连通性测试。

#### Traceroute
```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"tracert 10.0.0.2"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"tracert -a 10.0.0.1 10.0.0.2"}'
```
路由追踪。

### 路由策略

```bash
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display ip prefix-list"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display route-policy"}'
pyats_run_show_command '{"device_name":"h3c-core-01","command":"display as-path-acl"}'
```
路由策略配置。

---

## 工作流

### 1. H3C Comware OSPF健康检查

```
display ospf brief → OSPF进程、区域数
→ display ospf peer → 验证邻接关系Full状态
→ display ospf interface brief → 接口状态（DR/BDR/DROther）
→ display ospf lsdb summary → LSA数量
→ display ospf statistics → 错误计数、重传
→ 标记：非Full邻居、Cost不匹配、高重传
→ GAIT记录
```

### 2. H3C Comware BGP会话审计

```
display bgp peer → 邻居状态、前缀接收数、会话时间
→ display bgp peer {peer} verbose → 保持时间、AS号、策略
→ display bgp peer {peer} advertised-routes → 宣告路由
→ display bgp peer {peer} received-routes → 接收路由
→ 标记：非Established邻居、前缀数量异常、会话震荡
→ GAIT记录
```

### 3. H3C Comware IS-IS健康检查

```
display isis peer → 邻居状态
→ display isis interface → 接口状态
→ display isis lsdb summary → LSDB大小
→ display isis route → IS-IS路由
→ 标记：邻居Down、Level不匹配
→ GAIT记录
```

### 4. 路由表基线

```
display ip routing-table statistics → 各协议路由数量
→ display ip routing-table protocol ospf → OSPF路由
→ display ip routing-table protocol bgp → BGP路由
→ display ip routing-table protocol static → 静态路由
→ 对比NetBox/Nautobot前缀分配
→ GAIT记录
```

### 5. 端到端连通性测试

```
ping {target} → 基础可达性
→ ping -s 1500 {target} → MTU验证
→ tracert {target} → 路径分析
→ 对比 display ip routing-table {target} → 验证预期下一跳
→ GAIT记录
```

---

## 集成关系

| 技能 | 集成说明 |
|-----|---------|
| **h3c-comware-system** | 系统/硬件上下文（CPU、板卡状态）用于路由问题排查 |
| **h3c-comware-interfaces** | 接口状态补充路由协议分析 |
| **pyats-routing** | 相同方法论用于Cisco设备路由审计 |
| **pyats-parallel-ops** | pCall模式用于全网BGP/OSPF审计 |
| **te-path-analysis** | ThousandEyes路径数据补充traceroute分析 |
| **netbox-reconcile** | 对比学习路由与NetBox前缀分配 |
| **gait-session-tracking** | 所有命令执行记录到GAIT |

---

## 注意事项

- **所有命令为只读** — 仅执行show、display、ping、tracert命令
- **不做路由配置变更** — 不使用本技能进行路由协议配置
- **ping使用count限制** — 始终指定 `-c` 参数避免无限ping
- **与SoT对比** — 对比路由表与NetBox/Nautobot预期前缀
- **记录到GAIT** — 每次命令执行必须记录
