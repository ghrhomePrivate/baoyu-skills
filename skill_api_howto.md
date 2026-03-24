通过 API 调用 Claude Skills 是专业开发中实现自动化程序、大型项目部署的核心方式，核心依赖 Anthropic 的 **/v1/skills管理接口和Messages API 调用接口 **，且需要开启专属 Beta 能力、配合 Code Execution 环境，以下是完整实操步骤 + 核心示例 + 关键要求，覆盖技能的管理、调用全流程，适配 Python/TypeScript/cURL 主流开发方式：
一、前置准备（必做）
获取 Anthropic API Key：在Claude Console生成，需具备Skills 和 Code Execution Beta 权限（默认申请 API 后可开启，需在请求头指定 Beta 版本）；
环境要求：技能 API 依赖code-execution-2025-08-25（代码执行环境）和skills-2025-10-02（技能能力）两个 Beta 标识，所有 API 请求必须携带这两个标识；
技能前置：已有自定义技能文件夹（符合 kebab-case 命名、含 SKILL.md），或使用 Anthropic 官方技能（如 xlsx/pptx/pdf）。
二、核心 API 体系：两大接口搞定「技能管理 + 业务调用」
Claude Skills API 分为技能管理接口（创建 / 列出 / 管理技能）和消息调用接口（在实际业务中触发技能），前者负责维护技能生命周期，后者是生产环境核心使用方式。
（一）技能管理：/v1/skills 接口（Python/TypeScript/cURL 示例）
用于程序化创建、列出自定义技能，适合将技能集成到自动化部署流程（如 CI/CD 中自动更新技能版本）。
1. 列出所有技能（含官方 + 自定义）
Python
python
import anthropic
client = anthropic.Anthropic(api_key="你的ANTHROPIC_API_KEY")
# 列出所有技能
skills = client.beta.skills.list(betas=("skills-2025-10-02",))
for skill in skills.data:
    print(f"技能ID：{skill.id} | 名称：{skill.display_title} | 来源：{skill.source}")
# 仅列出自定义技能
custom_skills = client.beta.skills.list(source="custom", betas=("skills-2025-10-02",))
cURL
bash
curl "https://api.anthropic.com/v1/skills" \
  -H "x-api-key: 你的ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02"
2. 创建自定义技能（上传本地技能文件夹）
将本地符合规范的技能文件夹（如dcf-analysis/）上传为 Claude 的自定义技能，返回唯一skill_id（后续调用需用）。Python
python
from anthropic.lib import files_from_dir
client = anthropic.Anthropic(api_key="你的ANTHROPIC_API_KEY")
# 从本地文件夹创建技能
dcf_skill = client.beta.skills.create(
    display_title="DCF财务分析", # 技能显示名称
    files=files_from_dir("/本地路径/dcf-analysis"), # 技能文件夹路径
    betas=("skills-2025-10-02",)
)
print(f"自定义技能创建成功，ID：{dcf_skill.id}") # 保存该ID用于后续调用
（二）业务调用：Messages API 中嵌入技能（生产核心用法）
在常规的 Claude Messages API 请求中，通过 **container.skills参数 ** 指定要使用的技能（官方 / 自定义均可），即可让 Claude 在处理请求时自动加载并执行技能逻辑，这是自动化程序 / 大型项目中最常用的方式。
核心规则
container.skills是数组，可同时指定多个技能（符合 Claude 技能的可组合性）；
技能类型分两种：anthropic（官方技能，如 xlsx/pptx）、custom（自定义技能，需填创建时的skill_id）；
必须携带code-execution-2025-08-25和skills-2025-10-02两个 Beta 头。
实操示例：同时调用官方 xlsx 技能 + 自定义 DCF 分析技能
Python
python
import anthropic
client = anthropic.Anthropic(api_key="你的ANTHROPIC_API_KEY")
# 核心：在container.skills中指定技能
response = client.beta.messages.create(
    model="claude-sonnet-4-5-20250929", # 推荐使用最新Sonnet/Opus模型
    max_tokens=4096,
    # 必须携带两个Beta标识
    betas=("code-execution-2025-08-25", "skills-2025-10-02"),
    # 嵌入技能：官方xlsx + 自定义DCF分析
    container={
        "skills": [
            {"type": "anthropic", "skill_id": "xlsx", "version": "latest"}, # 官方技能
            {"type": "custom", "skill_id": "你的自定义技能ID", "version": "latest"} # 自定义技能
        ]
    },
    # 业务请求：让Claude执行技能对应的工作
    messages=[{"role": "user", "content": "基于提供的财务数据，用DCF模型做估值并生成Excel报表"}]
)
print(response.content[0].text)
cURL
bash
curl "https://api.anthropic.com/v1/messages" \
  -H "x-api-key: 你的ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {"type": "anthropic", "skill_id": "xlsx", "version": "latest"},
        {"type": "custom", "skill_id": "你的自定义技能ID", "version": "latest"}
      ]
    },
    "messages": [{"role": "user", "content": "基于提供的财务数据，用DCF模型做估值并生成Excel报表"}]
  }'
三、Claude Agent SDK 集成（大型项目推荐）
如果是构建自定义 AI Agent、自动化流水线等大型项目，推荐使用Claude Agent SDK（支持 Python/TypeScript），SDK 已封装 Skills 相关能力，无需手动处理接口细节，适配生产级的状态管理、子代理、批量处理等需求。
1. 安装 SDK 技能（快速集成）
bash
# 安装Python版SDK技能（Claude Code环境）
npx skills add waltersumbon/claude-agent-sdk-skills --skill claude-agent-sdk-python
# 或TypeScript版
npx skills add waltersumbon/claude-agent-sdk-skills --skill claude-agent-sdk-typescript
2. 核心优势
支持 ** 无状态query()和有状态client()** 两种调用模式，适配不同业务场景；
内置技能与 MCP 的协同逻辑，可直接在 Agent 中组合技能 + 外部工具调用；
提供完整的错误处理、类型提示，适合大型项目的工程化开发。
四、生产环境关键注意事项
版本管理：通过 Claude Console 可对技能做版本控制，调用时指定version（如v1.2）而非latest，避免线上环境因技能更新出问题；
权限控制：自定义技能可通过allowed-tools字段限制工具权限（如仅允许 Bash/Read），防止未授权的代码执行 / 外部调用；
隔离运行：复杂技能建议在 SKILL.md 的 YAML 中设置context: fork，让技能在隔离的子代理中运行，避免污染主会话、提高稳定性；
性能优化：同时启用的技能不超过 50 个，避免上下文过载；大型技能将详细文档放到references/文件夹，遵循渐进式披露原则；
日志与监控：监控 MCP/API 调用的错误码、重试率，通过 Claude Console 查看技能的触发次数、执行结果，及时排查问题。
五、适用场景（自动化 / 大型项目）
自动化程序：如财务自动化（调用 DCF 技能 + Excel 技能生成估值报表）、设计自动化（Figma 技能 + 前端设计技能生成开发文档）；
大型项目部署：在 CI/CD 流水线中调用技能，实现代码审查、自动化测试、部署报告生成；
自定义 Agent：基于 Claude Agent SDK 构建行业专属 Agent（如电商客服 Agent、金融分析 Agent），嵌入多个技能实现一站式能力；
企业级规模化部署：通过 API 将技能推送给企业所有应用，统一工作流程（如全公司使用相同的文档生成技能，保证格式规范）。
六、官方资源参考
Skills API 官方文档：Claude Skills API Quickstart
Agent SDK 文档：Claude Agent SDK
官方技能示例：anthropics/skills（GitHub）（可直接克隆修改使用）
问题排查：Claude Developers Discord 社区、GitHub Issues（anthropics/skills/issues）。
