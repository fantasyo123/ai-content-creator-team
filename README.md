# AI 内容创作专家团 · WorkBuddy Skill

> 把「AI 内容创作专家团」的完整方法论打包成可随身携带的 WorkBuddy Skill。
> 无论新开多少会话，加载本 Skill 即可调用完整的 6 角色多模态创作流水线。

---

## ✨ 特性

- **6 大专职角色**：策略策划（策凌）、文案创作（笔澜）、图像创作（珀西）、视频生成（维欧）、后期制作（柯立）、内容适配（艾达）
- **8 大预设工作流**：短视频流水线、图文批量生产、视频+图文物料、视频翻译本地化、文章+配图、脚本+成片一条龙、策略先行、发布级精修
- **平台原生工具**：直接调度 WorkBuddy 内置的 ImageGen / ImageEdit / VideoGen / 多模态生成能力
- **渐进披露设计**：SKILL.md 保持精简（275 词），6 个 references 按需加载，不占上下文
- **可兜底**：支持"子代理协作"与"单进程角色编排"双模式运行

## 📦 安装

方式一：**通过 zip 导入**

```bash
# 1. 下载本仓库 zip 或 clone
git clone https://github.com/<your-username>/ai-content-creator-team.git

# 2. 解压后放到你的用户 Skill 目录（Windows）：
#    %USERPROFILE%\.workbuddy\skills\ai-content-creator-team\
```

方式二：**手动复制目录**到 `~/.workbuddy/skills/`（macOS/Linux）或 `%USERPROFILE%\.workbuddy\skills\`（Windows）

---

## 🚀 快速上手

新开一个 WorkBuddy 会话，直接说：

| 你想要 | 对 WorkBuddy 说 |
|--------|-----------------|
| 宣传视频 | "生成一段产品宣传视频，风格 xx，竖屏" |
| 公众号文章+配图 | "写一篇带配图的公众号文章，主题是 xx" |
| 视频脚本+成片 | "帮我做个带讲解的短视频：写脚本→出关键帧→生成视频" |
| 品牌视觉策划 | "帮我设计一套咖啡店品牌视觉，先出创意方向" |
| 视频转文字 | "把这个 mp4 的对话转成文字" |
| 视频翻译 | "把这段中文视频翻译成英文" |
| 批量种草图 | "生成一组小红书种草图，风格统一，9 张" |

Skill 会自动路由到对应角色与工作流，执行完成后把图片 / 视频素材 / 成片 / 文档全部交付到产物面板。

---

## 🗂️ 目录结构

```
ai-content-creator-team/
├── SKILL.md                      # 入口：触发条件 + 路由总纲 + 决策速查
└── references/
    ├── team-members.md           # 6 个成员角色定义 / 工具白名单 / 输出格式
    ├── workflows.md              # 8 大 Workflow + 路由速查表
    ├── task-dispatch.md          # 任务下发五要素模板（正确/错误示例）
    ├── model-selection.md        # 平台模型选型决策表（集中维护点）
    ├── failure-handling.md       # 超时/拒绝/失败升级路径
    └── delivery-checklist.md     # 交付规范 + 交付前检查清单
```

---

## 🧠 团队角色与工作流（速览）

### 6 角色

| 角色 | ID | 擅长 |
|------|-----|------|
| 策凌 Celine | `creative-strategist` | 品牌定位 / 情绪板 / 广告方向 / 镜头设计 |
| 笔澜 Penn | `copywriter` | 公众号 / 小红书 / 脚本 / 文案 / SEO |
| 珀西 Percy | `image-creator` | 文生图 / 图生图 / 批量 / 精修 |
| 维欧 Veo | `video-generator` | 文生视频 / 图生视频 / 人物 / 特效 |
| 柯立 Clip | `video-editor` | 剪辑 / 拼接 / 字幕 / 滤镜 |
| 艾达 Ada | `content-adapter` | 视频分析 / 转录 / 翻译 / 合成 |

### 8 工作流

1. 短视频生产流水线
2. 图文内容批量生产
3. 视频+图文混合物料
4. 视频翻译与本地化
5. 文章 + 配图
6. 脚本 + 成片一条龙
7. 创意策划 + 内容生产（策略先行）
8. 发布级精修流水线

---

## ⚠️ 与「平台专家版」的差异

| 维度 | 平台专家版 | 本 Skill |
|------|-----------|----------|
| 成员类型 | 平台注册专属 Agent | 通用子代理 + 角色 Prompt |
| 维护 | 平台统一更新 | 自己维护（可 Fork 迭代） |
| 触发 | 会话选择专家 | 关键词自动触发 |
| 可分发 | ❌ | ✅ 随搬随用 |

核心流程、决策表、模板 100% 同源，仅"专属 Agent 类型"降级为通用子代理角色化。

---

## 🛠️ 迭代与自定义

- 调整模型：编辑 `references/model-selection.md`
- 调整角色职责：编辑 `references/team-members.md`
- 新增工作流：在 `references/workflows.md` 按现有格式追加
- 重打 zip：`python <skill-creator>/scripts/package_skill.py ./ai-content-creator-team ./dist`

---

## 📄 许可

MIT — 自由使用、修改、分发，保留署名请注明出处。