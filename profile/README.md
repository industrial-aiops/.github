# Industrial-AIOps

> **Governed, vendor-neutral AI ops for OT / 工控** — audit, budget, undo, and graduated approval built into every tool. Read-first, because the plant floor is exactly where you want an agent on a tight leash.

*[English](#english) · [中文](#中文)*

---

## English

AI agents are great at industrial operations — right up until one issues a write to a live PLC. **Industrial-AIOps** is a vendor-neutral OT data tap + cross-protocol troubleshooting layer for AI agents, wrapped in a **governance harness** so an agent's actions are:

- **Audited** — every operation logged to a local SQLite trail (who / what / why / when), secret-redacted.
- **Bounded** — per-process token/call budget + a runaway-loop breaker.
- **Reversible** — write/command tools record an inverse **undo token** (the before-value/state is captured).
- **Graduated** — risk tiers gate writes; the few OT-dangerous commands are off by default (`dry_run`), need a double-confirm, and a recorded approver (MOC).
- **Read-first** — the vast majority of tools only *read*; writes are the rare, heavily-gated exception.

Unlike the broader IT-ops family ([AIops-tools](https://github.com/AIops-tools)), this line is a **monorepo with a shared core + per-protocol connectors**, and a **menu-configurable MCP** — a site exposes only the 1–2 protocols it runs.

### Published

| Tool | Coverage | Install | Tools |
|------|----------|---------|:-----:|
| [**iaiops**](https://github.com/industrial-aiops/industrial-aiops) (0.10.0) | 14 field protocols: OPC-UA · Modbus-TCP/RTU · Siemens S7comm · Mitsubishi MC · Omron FINS · MTConnect · MQTT/Sparkplug B · Allen-Bradley EtherNet/IP · EtherCAT · PROFINET-DCP · **SECS/GEM (semiconductor / display fab)** · BACnet/IP (building) · HART-IP (process) · IO-Link — plus an AI downtime root-cause copilot, conservative baseline learning, ISA-18.2 alarm-flood analysis, historian read-back, a legacy PLC program explainer (ST/AWL/L5X), CSV/SQLite/Parquet export + a Prometheus/Grafana bridge, and 等保2.0/IEC 62443 compliance reporting | `pip install "iaiops[opcua,modbus]"` | 132 (123 read + 9 gated writes) |
| [**iaiops-energy**](https://github.com/industrial-aiops/industrial-aiops-energy) (0.1.3) | Energy edition (变电/电力), on top of `iaiops.core`: IEC 60870-5-104 · DNP3 · IEC 61850 MMS — read-only substation/RTU/IED monitoring, own MCP server identity + edition skill | `pip install "iaiops-energy[energy]"` | 8 protocol reads + mirrored brain |

### 🧪 Field-testing partners wanted

Everything software-verifiable is verified (in-process servers, real protocol libraries, container loopbacks); what remains on the honest `待核实` list only real equipment can answer — physical Modbus-RTU (RS-485), EtherCAT slaves, HART gateways, live BACnet HVAC, domestic PLCs (汇川/信捷), live PLCnext, live Omron FINS PLCs, IO-Link masters, substation RTUs/IEDs. Verified-equipment reports get credited in the support matrix; features are co-designed via Issues/Discussions. **Start here → [industrial-aiops#28 — Call for field-testing partners (v0.10.0)](https://github.com/industrial-aiops/industrial-aiops/issues/28)** (pinned).

### How it works

One package, one **menu-configurable** MCP server. Install only the protocols a site runs (`iaiops[opcua]`, or an edition bundle `iaiops[fab]`); expose only those at runtime (`IAIOPS_MCP=fab`). Config + the audit/undo/policy store live under `~/.iaiops/`; credentials are kept in an **encrypted store** (`secrets.enc`, Fernet + master password) — never plaintext.

```bash
pip install "iaiops[fab]"     # fab = secsgem + opcua + s7 + modbus
iaiops init                   # interactive setup (encrypts credentials)
iaiops doctor                 # verify connectivity (classified, not raw errors)
IAIOPS_MCP=fab iaiops mcp     # run as an MCP server, fab profile
```

Since 0.10.0 there is **no default tool exposure**: a bare `iaiops-mcp` prints the selection menu and exits — pick a profile (`IAIOPS_MCP=fab|factory|process|building|water|plcnext|brain|…`) or a pre-scoped `iaiops-mcp-<name>` entrypoint. Multi-process sites run 1 `iaiops-mcp-brain` + N protocol servers with `IAIOPS_MCP_NO_BRAIN=1`.

Also on the **[MCP Registry](https://registry.modelcontextprotocol.io)** (`io.github.industrial-aiops/iaiops`, `io.github.industrial-aiops/iaiops-energy`), **PyPI**, and **ClawHub**.

> ⚠️ **Honest validation status** — pure analysis, OPC-UA, Modbus-RTU (software serial), BACnet/IP reads, TDengine/IoTDB, DNP3 and IEC-61850 monitor paths are verified against real servers/libraries/containers; live field hardware largely stays `待核实` (see each repo's README). Read-first; never write to a production control system without authorization.

---

## 中文

AI agent 做工业运维很在行——直到它对在产 PLC 下了一次写。**Industrial-AIOps** 是给 AI agent 用的**厂商中立 OT 数据 tap + 跨协议智能排查**,外包一层**治理 harness**,让 agent 的动作:

- **可审计** —— 每次操作落本地 SQLite 流水（谁/做了什么/为什么/何时），密钥脱敏。
- **有预算** —— 每进程 token/调用预算 + runaway 熔断。
- **可回滚** —— 写/命令工具记录反向 **undo token**（捕获改前值/状态）。
- **分级审批** —— risk-tier 把写操作分级；少数 OT 危险命令默认关（`dry_run`）、需双重确认 + 记录审批人（MOC）。
- **只读优先** —— 绝大多数工具只**读**;写是罕见且重度受控的例外。

与偏 IT 的 [AIops-tools](https://github.com/AIops-tools) 不同,这条线是**共享 core + 按协议 connector 的 monorepo**,且 **MCP 可菜单配置**——现场只暴露它实际跑的 1–2 种协议。

### 已发布

| 工具 | 覆盖 | 安装 | 工具数 |
|------|------|------|:-----:|
| [**iaiops**](https://github.com/industrial-aiops/industrial-aiops)(0.10.0) | 14 种现场协议:OPC-UA · Modbus-TCP/RTU · 西门子 S7comm · 三菱 MC · 欧姆龙 FINS · MTConnect · MQTT/Sparkplug B · 罗克韦尔 EtherNet/IP · EtherCAT · PROFINET-DCP · **SECS/GEM(半导体/显示面板 fab)** · BACnet/IP(楼宇)· HART-IP(过程仪表)· IO-Link —— 外加 AI 停机根因 copilot、保守基线学习、ISA-18.2 报警洪泛分析、历史库读回、老 PLC 程序讲解(ST/AWL/L5X)、CSV/SQLite/Parquet 导出 + Prometheus/Grafana 桥、等保2.0/IEC 62443 合规报告 | `pip install "iaiops[opcua,modbus]"` | 132(123 读 + 9 门控写) |
| [**iaiops-energy**](https://github.com/industrial-aiops/industrial-aiops-energy)(0.1.3) | 能源版(变电/电力),基于 `iaiops.core`:IEC 60870-5-104 · DNP3 · IEC 61850 MMS —— 变电站 RTU/IED 只读监测,独立 MCP server 身份 + edition skill | `pip install "iaiops-energy[energy]"` | 8 个协议读 + 镜像脑 |

### 🧪 招募现场测试伙伴

软件里能验证的都验证了(in-process 服务器、真实协议库、容器 loopback);诚实的 `待核实` 清单只有真设备能回答——物理 Modbus-RTU(RS-485)、EtherCAT 从站、HART 网关、在线 BACnet 楼宇设备、国产 PLC(汇川/信捷)、真机 PLCnext、欧姆龙 FINS 真机、IO-Link 主站、变电站 RTU/IED。经你验证的设备会署名写进支持矩阵;功能通过 Issues/Discussions 共创。**参与入口 → [industrial-aiops#28 — 招募现场测试伙伴 (v0.10.0)](https://github.com/industrial-aiops/industrial-aiops/issues/28)**(置顶)。

### 怎么用

一个包、一个**可菜单配置**的 MCP server。现场只装它跑的协议（`iaiops[opcua]`,或行业捆绑 `iaiops[fab]`）;运行时只暴露这些(`IAIOPS_MCP=fab`)。配置与审计/undo/策略存于 `~/.iaiops/`;凭据放**加密存储**(`secrets.enc`,Fernet + 主密码),绝不明文。

```bash
pip install "iaiops[fab]"     # fab = secsgem + opcua + s7 + modbus
iaiops init                   # 交互式设置(加密凭据)
iaiops doctor                 # 连通自检(给归因结论,不是裸错误)
IAIOPS_MCP=fab iaiops mcp     # 以 fab profile 跑 MCP server
```

0.10.0 起**无默认工具暴露**:裸 `iaiops-mcp` 打印选择菜单后退出——用 `IAIOPS_MCP=fab|factory|process|building|water|plcnext|brain|…` 选 profile,或直接用预置入口 `iaiops-mcp-<name>`。多进程站点:1 个 `iaiops-mcp-brain` + N 个带 `IAIOPS_MCP_NO_BRAIN=1` 的协议 server。

同时在 **[MCP Registry](https://registry.modelcontextprotocol.io)**(`io.github.industrial-aiops/iaiops`、`io.github.industrial-aiops/iaiops-energy`)、**PyPI**、**ClawHub** 上架。

> ⚠️ **诚实验证状态** —— 纯分析、OPC-UA、Modbus-RTU(软件串口)、BACnet/IP 读、TDengine/IoTDB、DNP3 与 IEC-61850 监视路径已对真实服务器/库/容器验证;真实现场硬件大多仍 `待核实`(见各仓库 README)。只读优先;未经授权勿对生产控制系统写入。

---

## License & affiliation

MIT licensed. Community-maintained. **Not affiliated with, endorsed by, or sponsored by** the vendors or standards bodies of the systems these tools operate (Siemens, Rockwell Automation, Mitsubishi Electric, the OPC Foundation, SEMI, the Eclipse Foundation, etc.); all trademarks belong to their respective owners.
