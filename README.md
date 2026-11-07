# clarify-first

一个单文件的 **需求澄清** 技能（Skill），给 AI Agent 使用。

解决的问题很具体：Agent 常常在没搞清需求时就动手，做出来的东西方向不对，返工成本比多问两句高得多。这个技能给的是一套判断标准——什么时候该追问、什么时候该直接做、追问到什么程度收敛。

---

## 核心思路

不是"永远先问"，也不是"少废话直接干"，而是**先判断再决定**：

| 情况 | 处理 |
| --- | --- |
| 目标模糊 / 有多种合理做法 / 背景缺失 / 影响面大且不可逆 | 追问 |
| 指令具体 / 小而低风险 / 用户明确要求先出结果 / 紧急或纯探索 | 直接做 |

拿不准时不做完整访谈，只用一句话说"我需要先确认一个点"，然后问最关键的那个。

## 追问规则

- **一次只问一个问题**，不并列、不列清单
- 每个问题都针对当前**最大的不确定性**，不按固定清单机械推进
- 尽量转成 **2–4 个可选方向**，每个附一句代价或影响，降低回答成本
- 回答含糊就顺着同一点深挖，不跳话题
- 不重复问用户已经说过的；能自己查的先查（读代码、看文件）
- 通常 **3–5 轮收敛**；超过 5 轮仍不清晰，就先给理解摘要 + 显式假设 + 最小可行方案

## 需要摸清的五类信息

按优先级逐步覆盖，已明确的跳过：

1. **真实目标** — 最终想达成什么，为什么现在做
2. **背景与现状** — 现在怎么做、痛点在哪、有哪些既有条件
3. **约束** — 时间、技术栈、预算、合规、兼容性
4. **成功标准** — 什么算做成了，怎么验证
5. **边界** — 这次明确不做什么

## 收敛与交付

当你能用自己的话复述目标、背景、约束和验收标准时，停止追问：

1. 先写「理解确认」，复述目标、背景、约束、成功标准、不做什么
2. 再给落地方案：推荐做法、关键步骤、仍需拍板的取舍点
3. 若还剩一个真正无法判断的分叉，只就这一个提问，其余照常给方案

原则：**追问阶段不输出方案草稿**，也不为了显得高效而跳过澄清。

---

## 安装

### Trae / 兼容 `SKILL.md` 的客户端

全局安装：

```bash
mkdir -p ~/.trae-cn/skills/clarify-first
curl -fsSL https://raw.githubusercontent.com/<owner>/clarify-first/main/SKILL.md \
  -o ~/.trae-cn/skills/clarify-first/SKILL.md
```

Windows PowerShell：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.trae-cn\skills\clarify-first" | Out-Null
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/<owner>/clarify-first/main/SKILL.md" `
  -OutFile "$env:USERPROFILE\.trae-cn\skills\clarify-first\SKILL.md"
```

把 `<owner>` 换成仓库所属账号即可。也可以只装到单个项目：`.trae/skills/clarify-first/SKILL.md`。

> 若你同时使用多个 Agent 工具，可以把技能目录软链到各工具共享的位置，例如 `~/.agents/skills/`。

---

## 使用

装上后一般无需手动触发，Agent 遇到模糊需求时会自行加载。也可以显式要求：

> 先用 clarify-first 的方式问清楚，再动手

---

## 许可

[MIT](LICENSE)
