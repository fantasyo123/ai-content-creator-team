# 「内容创作专家团」转 Skill 拆解方案

> 目标：把当前运行的「AI 内容创作专家团」能力，转化为一个可随身携带、任意新会话可加载的 WorkBuddy Skill。
> 本方案供审阅确认，确认后按此实施。

---

## 一、方案定位

| 项 | 内容 |
|----|------|
| Skill 名称 | `ai-content-creator-team`（建议，可改） |
| 安装位置 | 用户级 `~/.workbuddy/skills/ai-content-creator-team/`（跨项目可用） |
| 触发词 | 生成视频、创作图文、做宣传片、写文案配图、AI 内容生产、多模态创作等 |
| 核心思路 | **固化"决策逻辑"，运行时用平台内置工具 + 通用子代理执行** |

关键差异：原专家是平台注册的角色 Agent；转成 Skill 后，成员分工改为"**通用子代理 + 角色定义 Prompt**"实现，执行结果接近，但边界靠指令约束。

---

## 二、Skill 目录结构

```
ai-content-creator-team/
├── SKILL.md                      # 入口：触发条件 + 路由规则 + 编排总纲（<5k 字）
├── references/
│   ├── team-members.md           # 6 个成员的角色定义/擅长领域/输入输出格式
│   ├── workflows.md              # 8 大 Workflow 完整流程图（含串行依赖）
│   ├── task-dispatch.md          # 任务下发五要素模板 + 正确/错误示例
│   ├── model-selection.md        # 视频/图像/理解模型选型决策表
│   ├── failure-handling.md       # 超时/拒绝/失败响应与升级路径
│   └── delivery-checklist.md     # 交付规范 + 交付前检查清单
└── assets/                       # （可选）示例脚本模板、成片样例等
```

**渐进披露设计**：SKILL.md 只放骨架；细粒度规则全部下沉到 references，按需加载，避免占用上下文。

---

## 三、SKILL.md 主体内容规划（骨架）

1. **Frontmatter**
   - `name: ai-content-creator-team`
   - `description:`（精准描述）触发词：视频生成、图片创作、文案、脚本、宣传片、品牌物料、图文内容等
   - `agent_created: true`（必须，供后续 SkillManage 修改/删除）

2. **角色定位**：创意制片人视角，负责拆解需求 → 匹配能力 → 编排成员 → 汇编交付。

3. **能力匹配速查表**（移植自专家版）：
   - 定位/情绪板/广告方向 → 策略成员
   - 公众号/小红书文案/脚本/口播 → 文案成员
   - 纯文生图/图生图/精修 → 图像成员
   - 文生视频/图生视频/人物/特效 → 视频生成成员
   - 剪辑/拼接/字幕/滤镜 → 后期成员
   - 视频分析/转录/翻译/合成 → 内容适配成员
   - ⚠️ 常见误派对照（写文章≠派图像成员、写脚本≠派视频生成成员等）

4. **模型选型决策表**：HY-Video-1.5 / YT-Video-2.0 / HumanActor / FX / HY-Image-V3.0 / Lite / ImageEdit / YT-VITA 的适用场景。

5. **编排规范**：
   - 8 大 Workflow 的路由判断（什么需求走哪条流程）
   - 串行依赖铁律（先写脚本→再关键帧→再成片）
   - 子任务命名规范（用角色名，禁用自创名）
   - 失败响应（临时错误重试 / 模型拒绝调整描述 / 工具不可用告知用户）

---

## 四、6 个成员角色迁移设计

| 成员 | 原平台 Agent 类型 | Skill 化实现 | AI 能力等效替换 |
|------|------------------|--------------|------------------|
| 策凌 (Celine) 策略 | creative-strategist | 子代理 + 策略角色 Prompt | 纯分析/输出，无专属工具，等效 |
| 笔澜 (Penn) 文案 | copywriter | 子代理 + 文案角色 Prompt | 纯文本，等效 |
| 珀西 (Percy) 图像 | image-creator | 子代理 + 图像角色 Prompt | 调 ImageGen/ImageEdit/多模态 Skill 产出图 |
| 维欧 (Veo) 视频生成 | video-generator | 子代理 + 视频角色 Prompt | 调 VideoGen（HY-Video-1.5 等）产视频 |
| 柯立 (Clip) 后期 | video-editor | 子代理 + 后期角色 Prompt | 用视频编辑编排 + ffmpeg 本地处理 |
| 艾达 (Ada) 内容适配 | content-adapter | 子代理 + 适配角色 Prompt | YT-VITA 分析 + ffmpeg/whisper 转录 |

每个成员角色定义包含：职责、输入输出规范、可调用的平台工具白名单、产出格式模板。

---

## 五、与专家版的行为差异（务必知悉）

| 维度 | 专家版 | Skill 版 |
|------|--------|----------|
| 成员类型 | 平台注册专属 Agent | 通用子代理 + 角色 Prompt（边界靠指令） |
| 维护方式 | 平台统一运营更新 | 用户自己维护版本 |
| 触发方式 | 会话选择专家 | 关键词/描述自动触发 Skill |
| 工具权限 | 平台托管 | 依赖平台内置工具，能力同源 |
| 分发 | 不可复制 | 可打包 zip 分发/导入 |
| 平台特性 | 主理人亲自编排 | 同样由调用方编排（流程一致） |

> 核心结论：**"流程/决策/模板"100% 迁移；"专属 Agent 类型"降级为通用子代理角色化**，仅此一项差异。

---

## 六、实施步骤（确认后执行）

1. **初始化**：`init_skill.py` 生成 `ai-content-creator-team` 骨架
2. **写 references**：逐个落地 team-matrix / workflows / task-dispatch / model-selection / failure-handling / delivery-checklist
3. **写 SKILL.md**：以精简 CVD 收编入口 + 路由图 + 编译规范
4. **自检**：pack 前内测「速查表命中 → 路由匹配 → 模板可套用」
5. **打包**：`package_skill.py` 出 zip → 交付
6. **试用**：请用户在真实需求（如"写一篇带配图的公众号文章"）上验证，收集反馈迭代

---

## 七、风险与降级说明

| 风险 | 降级策略 |
|------|----------|
| 智能体不识别子代理 | 提供两条运行模式：A. 子代理协作；B. 单进程内按角色步骤顺序完成（主代理代持多角色） |
| 平台模型名变化 | 模型名集中在 references/model-selection.md，改一处即可 |
| 平台专属工具更新 | 工具名集中、注明是平台内置（多模态生成 Skill、ImageGen、VideoGen） |
| 分散用户偏好 | 各平台尺寸/合规规则沉淀到 references，后续可随版本演进 |

---

*待你确认：Skill 名称、运行模式（子代理协作 vs 单进程角色化）、是否需要我直接按此方案落地实施。*