# theory-refine-loop-skill

一个跨 agent 通用 skill（ZCode / opencode / DeepSeek Harness）：把「审查 → 修改 → 再审查」闭环自动化——调用 [math-proof-reviewer](../) 对你的定理、证明、理论分析做苛刻审查，无问题直接返回「没问题」；有问题则按审查意见逐条修改后再次送审，反复循环直至通过；若在循环中发现理论本身站不住，停止循环并输出详细的失败原因报告。

## 功能

1. **送审** —— 每轮把最新工作副本交给 math-proof-reviewer，按四维度标准（前提充分性、引理使用、逻辑链条、边界与反例）审查；
2. **修改** —— 逐条对照审查意见补全论证、修正推导；只动理论内容，原文件不动（在 `*_revised` 副本上迭代，每轮留快照）；
3. **循环** —— 审查 → 修改 → 再审查，直至通过（上限 5 轮 + 顽固问题检测，防止无限循环）；
4. **通过** —— 明确回复「没问题」+ 最终确认要点 + 修改清单汇总；
5. **判死** —— 理论确实过不去时输出详细原因：完整反例 / 逻辑断层位置 / 假引理（引用 reviewer 原文）、每轮审查-修改记录、候选出路（削弱结论 / 补假设 / 换证明策略）；
6. **保护** —— 禁止靠悄悄弱化定理陈述换取通过；凡需改动结论本身的修改一律停下交用户决策。

## 依赖

- **math-proof-reviewer** skill 必须已安装（审查引擎），本仓库不包含它；
- 宿主 agent 需支持 Agent Skills 标准（`SKILL.md`）及 skill 加载工具。

## 安装

```bash
git clone https://github.com/openkills/theory-refine-loop-skill.git
cd theory-refine-loop-skill

# ZCode / DeepSeek Harness（默认扫描 ~/.agents/skills/）
mkdir -p ~/.agents/skills
cp -r theory-refine-loop ~/.agents/skills/

# opencode
mkdir -p ~/.config/opencode/skill
cp -r theory-refine-loop ~/.config/opencode/skill/

# DeepSeek Harness（显式目录，可选）
mkdir -p ~/.dsh/skills
cp -r theory-refine-loop ~/.dsh/skills/
```

## 使用示例

```text
把我的证明改到没问题：D:\papers\main.tex（审 Theorem 3.2）
检查理论分析并修改到通过：proof.docx
审到通过为止：section4-theory.md
```

## License

[MIT](LICENSE)
