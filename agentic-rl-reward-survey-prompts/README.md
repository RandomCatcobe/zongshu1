# Agentic RL Reward Survey Prompts

这个文件夹只保存可逐步投喂给 Codex 的提示词，不包含研究资料库本体。

## 使用顺序

按编号依次复制对应 `.md` 文件中的 prompt 给 Codex：

1. `prompt-00-init-repo.md`
2. `prompt-01-search-plan.md`
3. `prompt-02-first-source-collection.md`
4. `prompt-03-latest-2024-2026.md`
5. `prompt-04-extract-reward-info.md`
6. `prompt-05-part1-field-map.md`
7. `prompt-06-part2-reward-inventory.md`
8. `prompt-07-part3-cascade-drift.md`
9. `prompt-08-part4-silent-errors.md`
10. `prompt-09-part5-engineering-checklist.md`
11. `prompt-10-part6-gaps-antipatterns.md`
12. `prompt-11-part7-adjacent-fields.md`
13. `prompt-12-audit.md`
14. `prompt-13-final-synthesis.md`

## 最短路径

更现实的执行路径：

```text
Prompt 0
Prompt 1
Prompt 2
Prompt 3
Prompt 4
Prompt 7
Prompt 8
Prompt 10
Prompt 12
Prompt 13
```

也就是：先建资料库 -> 找源 -> 抽取 -> 深挖 cascade drift / silent errors / gaps -> 审计 -> 合成。

## 失败恢复

如果 Codex 跑偏，使用 `recovery-prompts.md` 中的纠偏 prompt。

