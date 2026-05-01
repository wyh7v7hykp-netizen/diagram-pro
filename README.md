# Diagram Pro — 融合版AI图表技能

> 🔥 **Fusion Skill**: 融合四大顶级AI Diagram Skill精华，为OpenClaw/Claude Code/Codex提供生产级图表生成能力。

## 融合基因

| 来源 | ⭐ | 注入的能力 |
|------|-----|-----------|
| 🔥 [fireworks-tech-graph](https://github.com/yizhiyanhua-ai/fireworks-tech-graph) | 5,000+ | 22种图表 + 7风格 + 语义形状/箭头 + SVG模板 |
| ✏️ [diagram-design](https://github.com/cathrynlavery/diagram-design) | 2,100+ | 编辑级设计哲学 + 60秒品牌提取 + Annotation原语 |
| 🎨 [excalidraw-skill](https://github.com/coleam00/excalidraw-diagram-skill) | 2,800+ | 视觉自检管道 + "Diagrams that argue"方法论 |
| 🔧 [drawio-skill](https://github.com/Agents365-ai/drawio-skill) | 800+ | OpenClaw原生metadata + Self-check循环 |
| 🇨🇳 [obsidian-visual](https://github.com/axtonliu/axton-obsidian-visual-skills) | 2,600+ | 中英双语触发 + 多输出模式 |

## 核心能力

- 🎨 **7种视觉风格**: Flat Icon / Dark Terminal / Blueprint / Notion Clean / Glassmorphism / Claude Official / OpenAI Official
- 📐 **22种图表类型**: 14种UML + 8种AI/Agent领域模式
- 🧠 **内置AI领域知识**: RAG、Agentic Search、Mem0、Multi-Agent等
- 🏷️ **60秒品牌onboarding**: 读网站→自动提取配色→统一所有图表
- 🔍 **视觉自检管道**: 生成→渲染→检查→修复循环（最多3轮）
- ✍️ **编辑标注原语**: 斜体衬线注释 + 虚线引导线
- 🇨🇳 **完整中文支持**: 中文触发词 + 中文排版规则 + 中文品牌模板

## 快速开始

```bash
# 安装依赖
brew install librsvg

# 复制到OpenClaw技能目录
cp -r diagram-pro ~/.openclaw/skills/diagram-pro

# 或复制到Claude Code技能目录
cp -r diagram-pro ~/.claude/skills/diagram-pro
```

### 使用示例

```
# 基本用法
User: "画一个RAG管线流程图，深色风格"
→ 生成: rag-pipeline.svg + rag-pipeline.png

# 品牌onboarding
User: "onboard diagram-pro 到 https://mysite.com"
→ 自动提取品牌色 → 所有后续图表使用你的品牌色

# 中文自然语言
User: "帮我把微服务架构画出来，用蓝图风格"
→ 生成: microservices.svg + microservices.png

# 带annotation
User: "画个Multi-Agent协作图，标注为什么不用中心化调度"
→ 图 + 编辑标注解释设计选择
```

## 设计哲学

> 每张图都是一篇视觉论述，不是一堆形状的堆砌。

- **删减至上** — 最高质量的操作通常是删除
- **一眼焦点** — 强调色只留给最重要的1-2个元素
- **论证而非展示** — 形状映射概念

## 许可

MIT License. 基于 fireworks-tech-graph (MIT) 融合构建。
