# 研发模式卡片增强

让「研发模式」「研发·侦察」「研发·验收」三张卡片与内置模式显示一致：卡片底部出现「模式说明 / 如何使用」两个按钮，点开是可滚动、可切换两个标签页的帮助对话框。

## 为什么需要它

内置四个模式（标准 / PTC / 极简 / 创造）的那两个按钮来自 DSH 客户端的硬编码：

- `@deepseek-ai/dsh-client-ui-agent-preset/src/client/PresetGuideDialog.tsx` 的 `guides` 表只登记了 `standard`、`ptc`、`minimal`、`cordis` 四个 id，且 `presetGuide(id, trust)` 对 `trust !== 'system'` 直接返回 `undefined`；
- 预设声明可用的字段只有 `id / name / description / order / plugins`，没有承载指南文案的字段。

因此第三方预设无法通过配置获得这两个按钮，只能由客户端插件替换那一格设置页。

## 实现方式

`client.js` 是从**已安装的 DSH** 里提取的 `@deepseek-ai/dsh-client-ui-agent-preset` 浏览器产物（版本见文件头注释），由 `scripts/vendor-preset-cards.mjs` 生成，做了四处改动：

1. 在 `guides` 表里追加 `rd`、`rd-scout`、`rd-review` 三条；
2. 放宽 `presetGuide` 的判定，让这三个 id 在 `user` 信任下也能取到指南；
3. 改用自有 locale 命名空间 `settings.rdPreset`，与内置插件互不冲突；
4. 去掉新会话芯片、会话头标签、插件菜单三项注册，只保留 `settings.section` 里 `agent-presets` 这一格——按 DSH 槽位约定，复用内置 id 会替换该格。

内置四个模式的卡片仍由同一份组件渲染，展示与行为不变。

## 维护

- **DSH 升级后**：在本仓库运行 `node scripts/vendor-preset-cards.mjs` 重新生成 `client.js`；脚本的锚点不匹配时会报错，表示内置界面已变化，需要重新推导补丁。
- **停用**：`plugin_manager set_bundle`，target 为 `@local/dsh-rd-mode-cards`，enabled 为 `false`。
- **移除**：`plugin_manager remove_bundle`。
- 浏览器端需要刷新页面才能看到变化；这是 Web 界面插件，不影响会话与预设本身。

## 授权与归属

`client.js` 是从 `@deepseek-ai/dsh-client-ui-agent-preset`（MIT License，DeepSeek）提取并修改的产物，版权归原作者所有；本仓库仅做上述四处本地改动。
