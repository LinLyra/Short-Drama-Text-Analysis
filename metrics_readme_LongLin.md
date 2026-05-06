# 指标说明（无 LLM 版）
*文件*: `processed_conversations.csv`, `dataprocessing_notebook_林珑.ipynb`  
*姓名*: 林珑  

---

## 0. 术语
- 练习/会话：一次员工与 AI 的完整练习（`practice_id`）。  
- 轮次 turn：一条消息。USER 为员工侧，ASSISTANT 为 AI 侧。  
- TTR：Type-Token Ratio，词类型数/总词数。  
- Jaccard：集合交并比 \( |A∩B| / |A∪B| \)。

---

## 1) 基础信息
| 列名 | 含义/计算 | 单位/范围 | 备注 |
|---|---|---|---|
| `practice_id` | 练习唯一 ID | - | 主键 |
| `practice_time` / `practice_dt` | 开始时间；`practice_dt` 为解析后的时间戳 | 日期时间 | 导出时已转字符串 |
| `practice_duration_min` | 持续时长（毫⽐转分钟） | 分钟 | round(…,3) |
| `employee_uid` / `employee_name` / `employee_department` | 员工标识、姓名、部门 | - | - |
| `scene_title` / `scene_description` / `scene_objective` | 场景标题/描述/目标 | - | 源自原始 JSON |
| `total_score` / `total_score_num` | 评分类原值/转数值 | 分 | 可能缺失 |
| `evaluation` | 评语 | 文本 | 可能缺失 |

---

## 2) 互动结构
| 列名 | 含义/计算 | 单位/范围 | 解释 |
|---|---|---|---|
| `turns_total` | 全部轮次 | 次 | - |
| `turns_user` / `turns_assistant` | 用户/AI 轮次 | 次 | - |
| `role_balance_user_over_asst` | 轮次比 = 用户轮次 ÷ AI 轮次 | ≥0，通常 0.3~3 | ≈1 为均衡，<1 AI 主导，>1 用户主导 |

---

## 3) 文本长度与提问率
| 列名 | 含义/计算 | 单位/范围 | 解释 |
|---|---|---|---|
| `avg_len_tokens_all/user/asst` | 平均**词数**（全体/用户/AI） | 词/条 | 空格分词 |
| `avg_len_chars_all/user/asst` | 平均**字符数**（全体/用户/AI） | 字/条 | - |
| `question_rate_all` | 含“?/?？”消息占比（全体） | 0~1 | - |
| `question_rate_user/asst` | 用户/AI 的提问率 | 0~1 | - |

---

## 4) 词汇多样性（TTR）
| 列名 | 含义/计算 | 单位/范围 | 解释 |
|---|---|---|---|
| `ttr_all` / `ttr_user` / `ttr_asst` | TTR = (去重词数/总词数) | 0~1 | 越高词汇越丰富 |

---

## 5) 频率与节奏 & 学习轨迹
| 列名 | 含义/计算 | 单位/范围 | 解释 |
|---|---|---|---|
| `gap_since_prev_hours` | 与上次练习时间间隔 | 小时 | 员工内按时间排序 |
| `daily_practice_count` | 当天员工练习次数 | 次 | - |
| `weekly_practice_count` | 当周员工练习次数 | 次 | 周起止按系统默认 |
| `session_idx` | 员工内第几次练习（从1计） | 次序 | 练习轨迹 |
| `score_delta_prev` | 分数提升 = 本场分数 - 上场分数 | 分 | 可正可负 |
| `score_ma3` | 分数 3 次滚动均值（员工内） | 分 | 不足3次取已有均值 |

---

## 6) 对话新颖性（词面/可选语义）
| 列名 | 含义/计算 | 单位/范围 | 解释 |
|---|---|---|---|
| `novelty_1_minus_maxsim` | 1-与历史“最像的一次”相似度（TF‑IDF/Jaccard） | 0~1 | 越大越新 |
| `novelty_embed_gemini_1_minus_maxsim` | 1-与历史“最像的一次”**语义相似度**（Embedding） | 0~1 | 越大越新，**可选** |

> 新颖性计算基于**同一员工**过去的练习（时间早于当前）。没有历史时记为 1.0。

---

## 7) 互动与可读性补充
| 列名 | 含义/计算 | 单位/范围 | 解释 |
|---|---|---|---|
| `alternation_rate` | 角色切换次数 ÷ 轮次 | 0~1 | 越高互动越“你一句我一句” |
| `assistant_token_share` | AI 词数 ÷（用户+AI 词数） | 0~1 | >0.6 AI主导，<0.4 用户主导 |
| `asst_adjacent_jaccard_mean` | AI 相邻两轮 Jaccard 相似度均值 | 0~1/NaN | 越高越“模板化/重复” |


