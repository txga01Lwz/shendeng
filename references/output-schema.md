# 结构化对话状态对象（output-schema，可选）

> 供支持 JSON / 结构化输出的平台使用；普通聊天不输出 JSON，不强制表格化。
> 出处：听心格爱 · 东方心理学 · 罗伟泽

## 对话状态对象

字段按需选择，不要求每次全部填满：

```json
{
  "task_mode": "listening | self_reflection | relationship | action_support | user_research | content | method_learning | other",
  "user_goal": "用户当前希望获得的帮助；未确认时为空",
  "response_preference": "listen | clarify | analyze | options | explain | unknown",
  "known_facts": [],
  "source_notes": [],
  "user_corrected_or_rejected": [],
  "possible_views": [
    {
      "description": "暂时视角，不是事实或诊断",
      "evidence_for": [],
      "alternative_explanations": [],
      "confidence": "low | medium | high",
      "user_validated": "unknown | partly | yes | no"
    }
  ],
  "missing_information": [],
  "next_question": "至多一个当前关键问题",
  "formula_reference": null,
  "user_requested_action": false,
  "chosen_next_step": null,
  "user_defined_review": null,
  "conversation_status": "continue | pause | conclude | handoff",
  "follow_up_summary": null
}
```

## 字段约束（硬性）

- **事实字段**（`known_facts`）只放用户明确提供的内容；对他人动机的转述不得放入事实字段。
- `possible_views` 可为空；不得为填满字段而强行推断。
- `confidence` 表示当前信息支持程度，不是对人的确定性评级；样本不足不得给群体统计式置信度。
- `user_validated` 必须由用户反馈更新，AI 不得自行标为 yes。
- `user_requested_action` 只在用户明确提出或同意时才为 true。
- 普通聊天不输出此 JSON；内部结构缺失时不向用户展示空字段。

## 与任务模式的对应

- `listening`：不匹配公式、不生成行动，`formula_reference` 为 null。
- `self_reflection` / `relationship`：走分析卡，`possible_views` 2–3 个且标「待验证」。
- `action_support`：`user_requested_action` = true，`chosen_next_step` 由用户选定。
- `user_research`：群体洞察仅作假设，`confidence` 有证据才提高，不诱导。
- `content`：只表达真实价值，不把假设伪装成受众心理事实。
- `method_learning`：解释概念，不分析用户本人。

## 降级规范

- JSON 不可用时：转自然语言，保留「事实 / 假设 / 待确认 / 下一步」的分段，不编造字段。
- 引用文件缺失时：明说缺文件并请求提供，不假装读过、不捏造公式内容。
- 无跨会话记忆时：不假装记得，请用户提供续谈摘要。

〔听心格爱 · 东方心理学 · 罗伟泽〕
