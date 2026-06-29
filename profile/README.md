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
| [**iaiops**](https://github.com/industrial-aiops/industrial-aiops) | OPC-UA · Modbus-TCP · Siemens S7comm · Mitsubishi MC · MTConnect · MQTT/Sparkplug B · Allen-Bradley EtherNet/IP · EtherCAT · **SECS/GEM (semiconductor / display fab)** — plus cross-protocol diagnostics (no-data dataflow, OPC-UA connection self-diagnosis, subscription health, ISA-18.2 alarm bad-actors, tag/historian health) and OEE/downtime + asset-inventory analytics | `pip install "iaiops[opcua,modbus]"` | 66 |

### How it works

One package, one **menu-configurable** MCP server. Install only the protocols a site runs (`iaiops[opcua]`, or an edition bundle `iaiops[fab]`); expose only those at runtime (`IAIOPS_MCP=fab`). Config + the audit/undo/policy store live under `~/.iaiops/`; credentials are kept in an **encrypted store** (`secrets.enc`, Fernet + master password) — never plaintext.

```bash
pip install "iaiops[fab]"     # fab = secsgem + opcua + s7 + modbus
iaiops init                   # interactive setup (encrypts credentials)
iaiops doctor                 # verify connectivity (classified, not raw errors)
IAIOPS_MCP=fab iaiops mcp     # run as an MCP server, fab profile
```

Also on the **[MCP Registry](https://registry.modelcontextprotocol.io)** (`io.github.industrial-aiops/iaiops`), **PyPI**, and **ClawHub**.

> ⚠️ **Preview** — validated against simulators / mocks, **not** live equipment. Read-first; never write to a production control system without authorization.

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
| [**iaiops**](https://github.com/industrial-aiops/industrial-aiops) | OPC-UA · Modbus-TCP · 西门子 S7comm · 三菱 MC · MTConnect · MQTT/Sparkplug B · 罗克韦尔 EtherNet/IP · EtherCAT · **SECS/GEM(半导体/显示面板 fab)** —— 外加跨协议诊断（无数据定位、OPC-UA 连接自检、订阅健康、ISA-18.2 报警风暴、tag/historian 健康）与 OEE/停机 + 资产盘点分析 | `pip install "iaiops[opcua,modbus]"` | 66 |

### 怎么用

一个包、一个**可菜单配置**的 MCP server。现场只装它跑的协议（`iaiops[opcua]`,或行业捆绑 `iaiops[fab]`）;运行时只暴露这些(`IAIOPS_MCP=fab`)。配置与审计/undo/策略存于 `~/.iaiops/`;凭据放**加密存储**(`secrets.enc`,Fernet + 主密码),绝不明文。

```bash
pip install "iaiops[fab]"     # fab = secsgem + opcua + s7 + modbus
iaiops init                   # 交互式设置(加密凭据)
iaiops doctor                 # 连通自检(给归因结论,不是裸错误)
IAIOPS_MCP=fab iaiops mcp     # 以 fab profile 跑 MCP server
```

同时在 **[MCP Registry](https://registry.modelcontextprotocol.io)**(`io.github.industrial-aiops/iaiops`)、**PyPI**、**ClawHub** 上架。

> ⚠️ **预览版** —— 仅对模拟器/mock 验证,**未**对真实设备验证。只读优先;未经授权勿对生产控制系统写入。

---

## License & affiliation

MIT licensed. Community-maintained. **Not affiliated with, endorsed by, or sponsored by** the vendors or standards bodies of the systems these tools operate (Siemens, Rockwell Automation, Mitsubishi Electric, the OPC Foundation, SEMI, the Eclipse Foundation, etc.); all trademarks belong to their respective owners.
