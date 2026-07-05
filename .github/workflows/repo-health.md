---
on:
  workflow_dispatch:

engine:
  id: copilot
  model: gpt-5.3-codex

permissions:
  contents: read
  copilot-requests: write

network: defaults

safe-outputs:
  create-issue:
  add-comment:

---

# diagram-pro 仓库健康分析

对整个仓库进行全面的健康状况评估，生成改进建议报告。

## Instructions

全面分析这个仓库，从以下几个维度出具评估报告：

### 1. 项目结构
- 目录结构是否合理
- Python 包结构是否规范（有无 __init__.py、setup.py/pyproject.toml）
- Shell 脚本是否集中管理

### 2. 代码质量
- 扫描 Python 脚本的长度和复杂度
- Shell 脚本是否有明显的安全问题（硬编码密钥、不安全的临时文件）
- 检查是否有重复代码或死代码
- 代码风格是否一致

### 3. 文档健康度
- README 是否完整
- SKILL.md 是什么角色，是否与其他文档有信息重叠
- 中英文 README 是否同步
- 是否有缺失的 API 文档或使用示例

### 4. 依赖与配置
- package.json 中的依赖是否合理
- 是否有 requirements.txt 或 pyproject.toml（Python 依赖管理）
- .gitignore 和 .npmignore 是否覆盖了必要的内容

### 5. 改进建议
- 列出前 5 个最值得改进的问题
- 每个问题标注严重程度（高/中/低）
- 给出具体的改进建议

最终输出一份结构化的 markdown 报告，创建为一个 issue 提交。
