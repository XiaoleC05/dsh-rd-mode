# 变更日志

格式：版本｜日期｜变更等级（C1 技能内容 / C2 宪章与配置 / C3 插件与宿主）｜改动｜预期影响的 KPI｜来源（提案编号或初始设计）。
版本规则：C1 递增补丁号；C2、C3 递增次版本号。

## 0.1.0｜2026-11-19｜初始版本
- 新增三个预设：rd（研发模式）、rd-scout（研发·侦察）、rd-review（研发·验收）。基线为 DSH 0.2.2-rc.5 官方 standard 预设。
- 宪章：任务分级 L0–L3、流水线 S1–S8、验证与独立验收两道关卡、交付报告模板、Token 纪律、安全红线；suffix 为交付前自检。
- 配置：压缩阈值约 199k（thresholdRatio 0.19、retainTokens 40000）；read 单次 32 KB；grep 150 条 / 每行 600 字节；网页抓取 80k 字符；项目规则文件 24 KB / 链；子代理 effort=high，review_agent effort=max。
- 去掉：PTC 执行器、workflow、schedule、subagent_fork、list_subagent_models（理由见 registry.md）。
- 技能：rd-code-standards、rd-testing、rd-web（含落地页文案标准与上线检查表）、rd-data、rd-ai、rd-ecosystem、rd-retro、rd-selftest。
- 工具：scripts/validate-bundle.mjs + validate.ps1（完整校验）、scripts/rd-kpi.mjs（KPI 统计）。
- 预期：K1 平均每次请求上下文从 319,202 降到 ≤ 120,000；K3 超过 200k 的请求从 66.2% 降到 ≤ 5%；K9–K11 流程合规 ≥ 90%。
- 发布前经三路独立复核（DSH 事实核查、按 INIT 模拟执行、宪章对抗审查），共 60 条意见、无阻断项，已修订：§8 红线优先于用户指令且明确“点名即同意”；项目规则只在风格与命令上优先；L3 规格卡与计划一次批准、计划模式下统一用 exit_plan_mode；验收结论增加 UNVERIFIED，复审改用 send_message；子代理约定（不分级、不委派、不提问、不提交）；长任务改为前台运行并一次等待；不再重复读取已注入的 AGENTS.md；L0/L1 不加载技能；GBK/UTF-16 文件的读取例外；KPI 改按任务段统计并加回归测试；复盘不读取会话原文；INIT 补全各分支与用户确认步骤。
- 来源：docs/DESIGN.md。安装前已在隔离的 DSH 0.2.2-rc.5 宿主中验证三个预设均激活成功（无 broken）。
