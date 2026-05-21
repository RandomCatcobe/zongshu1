# 失败恢复 Prompts

## 证据不足时纠偏

```text
停止扩写。你现在的问题是开始在没有证据的情况下写结论。

请回到文件化 workflow：
1. 先检查 sources/sources.jsonl 是否有对应 source。
2. 没有 source 的 claim 标记为 unsupported。
3. 把 unsupported claim 写入 audits/claim_audit.md。
4. 正文中没有证据的地方改成“此处覆盖不足”或“hypothesis”。
5. 不要继续生成新结论，直到 source 补齐。
```

## 防止一次性输出全文

```text
不要在聊天窗口输出完整综述。所有长内容必须写入 drafts/*.md 文件。当前步骤只允许完成指定文件。完成后停止，并报告文件路径、完成状态、缺失项。
```

## 搜索不到某个工作

```text
不要编造。请把该工作写入 sources/missing_sources.md，字段包括：
- name
- attempted queries
- why it matters
- what information is missing
- next search suggestion

然后继续处理其他已找到 source。
```

