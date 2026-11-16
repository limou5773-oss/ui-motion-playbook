# ui-motion-playbook

一套 UI 动效与交互设计规范，写成 AI Agent 可直接加载的 skill（Agent Skills 开放格式）。

**来源**：6 个动效教学视频（逐条语音转写 + 抽帧核对）＋ Carbon Design System 官方规范 ＋ 实测参数。每个数字都能反查到出处。

## 它解决什么问题

让 AI 做出"不像 AI 做的"界面。核心不是加更多动画，而是：

1. **先判断该不该动**（动效三问）
2. **动就给出数字**（毫秒、像素、透明度、曲线），不用"丝滑""高级"这类词
3. **有硬闸门**（无障碍、性能、测试环境）

> 「叫得出名字，AI 才改得对。」

## 结构

```
ui-motion-playbook/
├── SKILL.md                     入口：六步工作流 + 触发词
├── references/
│   ├── effects-catalog.md       16 种动效 + 6 种遮罩层 + 5 种加载形态
│   ├── motion-spec.md           来源与置信度、时长/缓动表、四条按钮提示词原文
│   ├── ai-prompting.md          「跟 AI 说」句式库、反模式对照
│   ├── review-checklist.md      视觉规则原文、去 AI 味四条、三层自检
│   ├── admin-navigation.md      后台菜单六种回应 + 滑动高亮块实现
│   └── transcripts.md           6 份完整转写稿（未删改，带时间戳）
├── assets/                      4 张原始截图
└── adapters/                    其他 agent 的适配文件
```

## 安装

### WorkBuddy

```bash
cp -r ui-motion-playbook ~/.workbuddy/skills/
```

调用：`/ui-motion-playbook`，或说「给这个页面加点动效」时按 description 自动触发。

### Claude Code（原生支持，零改造）

```bash
cp -r ui-motion-playbook ~/.claude/skills/          # 个人级
cp -r ui-motion-playbook <项目>/.claude/skills/      # 项目级，可提交共享
```

### Codex

```bash
mkdir -p ~/.codex/prompts
cp adapters/codex/prompts/ui-motion.md ~/.codex/prompts/
# 重启 Codex，输入 /ui-motion
```

想让规则常驻，把 `adapters/codex/AGENTS.md.snippet` 追加到项目根 `AGENTS.md`。

### Cursor / Cline

```bash
cp adapters/.cursor/rules/ui-motion.mdc <项目>/.cursor/rules/
cp adapters/.clinerules/ui-motion.md    <项目>/.clinerules/
```

更多细节见 `adapters/README.md`。

## 核心内容速览

### 动效三问（不清就不加）

1. 是否表达了一次状态变化？
2. 是否缩短了等待的感知？
3. 是否提供了空间定位？

三条全否 → 不加。这一问能筛掉大部分「AI 味」。

### 三条硬指标

| 项 | 值 |
|---|---|
| 时长档位 | 微交互 90-140ms / hover 150-200ms / 入场 200-300ms / 弹窗 200-260ms |
| 只动 | `transform`、`opacity`、`filter`（必须动高度用 `grid-template-rows: 0fr → 1fr`） |
| 必须支持 | `prefers-reduced-motion: reduce` |

### 视觉规则（可直接抄进提示词）

```
配色：中性色为主，用明暗对比突出主要操作。强调色可选，最多一种。禁用渐变、发光和彩色阴影。
间距：仅用 4 8 12 16 24 32 px。组内间距小于组间间距。
圆角：仅用 6 10 16 px。嵌套圆角：外圆角 = 内圆角 + 内边距。
对齐：同一列元素共用左边缘。
内容：统一一套线性图标。不要表情符号、感叹号或装饰性徽章。
```

## 来源与置信度

| 来源 | 内容 | 置信度 |
|---|---|---|
| 6 条视频（转写 + 抽帧核对） | 效果清单、判据、口诀、四条按钮提示词原文 | 高（原话） |
| Carbon Design System — Motion | 时长令牌 70/110/150/240/400/700ms、三条缓动曲线 | 高（官方规范） |
| 标注 `≈` 的区间 | 骨架屏周期、跑马灯速度、加载条周期 | 低（经验值，可调） |

所有数字在 `references/motion-spec.md` 的「来源与置信度」表中可反查；原始转写稿在 `references/transcripts.md`。

## 已知缺口

第 7 个视频「想要做加载动画怎么跟 AI 说」源文件始终未获取到，加载动画那一节是通用做法补充，不是原话。
