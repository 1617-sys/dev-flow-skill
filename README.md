# dev-flow

一个精简、风险驱动的 Agent 开发工作流 Skill。它把简短需求补全为可执行任务，并独立判断风险档位、OpenSpec、审批门禁和验证强度。

当前版本：1.0.0-rc.2

## 设计目标

- 充分利用现代底模和 Harness，不堆叠重复工作流插件。
- 按风险、影响范围、可逆性和耦合度分类，而不是按代码行数分类。
- 风险档位与 OpenSpec 解耦。
- Quick 任务保持轻量，Strict 任务使用机械只读与审批门禁。
- 默认单代理、当前工作区；只有收益明确时才增加隔离或独立评审。

## 三档工作流

| 档位 | 典型任务 | 默认流程 |
|---|---|---|
| Quick | 样式、文案、机械修改、局部恢复既有行为 | 直接执行，最小相关验证 |
| Standard | 新增用户功能、普通 Bug、共享逻辑或跨模块可逆改动 | 短计划后实施，回归与工程检查 |
| Strict | 权限、安全、迁移、公共契约、不可逆操作 | 只读调查，明确批准后实施 |

OpenSpec 不由风险档位自动决定。比如恢复既有权限规范属于 Strict + no OpenSpec；改变权限规则本身才需要 OpenSpec。

## 安装

安装后应满足：

    <skills-directory>/
    └── dev-flow/
        ├── SKILL.md
        └── references/
            └── scenario-tests.md

### Codex

Windows PowerShell：

    git clone https://github.com/1617-sys/dev-flow-skill.git "$HOME\.codex\skills\dev-flow"

macOS / Linux：

    git clone https://github.com/1617-sys/dev-flow-skill.git ~/.codex/skills/dev-flow

重启 Codex 或开启新任务后生效。

### Claude Code

    git clone https://github.com/1617-sys/dev-flow-skill.git ~/.claude/skills/dev-flow

如果你的 Harness 使用其他个人 Skill 目录，只需保证 SKILL.md 位于 dev-flow/ 的直接下级。

## 使用

一般开发请求可以自动触发，例如：

    修复搜索结果偶尔重复的问题。

需要显式调用时：

    $dev-flow 为设置页面增加主题切换。

跨 Harness 的通用写法：

    按 dev-flow 处理：修改 CSV 导出字段。

Skill 会在内部补全目标、范围、验收标准、风险和验证路径，不会默认向用户展示冗长的 Prompt 重写。

## OpenSpec

OpenSpec 是可选的规格记录层，不是本 Skill 的安装依赖。只有公共契约、持久化结构、安全规则、稳定产品的既有行为等值得长期维护的变化才会触发它。

## 验证

references/scenario-tests.md 包含维护时使用的回归矩阵。当前 RC 版本已完成 21 个规则场景和 2 个自动触发黑盒场景测试。

## License

[MIT](LICENSE)
