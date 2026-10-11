# Monash Moodle Skill

一个独立的 Codex Skill，用自然语言查询 Monash Moodle 的 due、成绩、考试要求和考勤规定，并指导登录恢复。

依赖 [bunizao/moodle-cli](https://github.com/bunizao/moodle-cli)，也能使用已经连接的 Moodle MCP 或经授权的浏览器。此项目与 Monash University 无官方隶属关系。

## 与上游的关系

上游提供 CLI、MCP、会话管理和原始 Skill。本项目提供面向 Monash 学生的查询流程，重点增加：

- 按课程和教学期解析简称，避免硬编码个人课程 ID。
- due 查询范围、列表完整性、时区与提交状态的区分。
- 考试要求和考勤规定的多来源核对；缺少证据时明确标注。
- CLI / MCP / 网页读取方式的透明说明。
- 登录失效、macOS Security 弹窗和 Cloudflare 初次部署错误的针对性处理。

没有修改或打包上游 CLI 源码。CLI 命令依据本机 0.10.0 的帮助和上游文档编写；不同版本使用前以实际 `--help` 为准。

## 安装

先按上游说明安装 `moodle-cli`。已有可用 Moodle MCP 的环境也可以直接使用已连接的工具。

下载或克隆本仓库后，将 `skills/monash-moodle` 整个文件夹复制到 Codex 用户 Skill 目录，通常是 `~/.codex/skills/monash-moodle`；设置了 `CODEX_HOME` 时使用其下的 `skills/monash-moodle`。若目标目录已存在，先比较或备份，避免覆盖自定义内容。重新打开会话并确认 Skill 可见。

本仓库是可分发源文件，不会自动安装 CLI、创建 Cloudflare Worker 或复制登录状态。

首次连接本机 CLI 时运行：

```bash
MOODLE_BASE_URL=https://learning.monash.edu moodle auth login
MOODLE_BASE_URL=https://learning.monash.edu moodle user --json
```

在学校登录页面完成 SSO/MFA。会话有效时通常不必每次重新登录；过期或被撤销后仍需重新认证。MCP 的连接与登录恢复流程见 Skill 内的访问指南。

## 使用示例

- `用 $monash-moodle 检查未来两周 due。`
- `查一下我这学期 1001 的期末考试要求，给出来源。`
- `看看期中考试成绩；还没有公布的请单独注明。`
- `检查当前四门课有没有考勤或 participation 要求。`
- `Moodle CLI 又弹出 macOS Security，帮我判断是哪种授权。`

Skill 默认允许自动匹配相关 Monash 查询。若同时安装上游 `moodle-cli` Skill，可用 `$monash-moodle` 明确选择本流程。课程简称存在歧义时会先核对课程列表。

## 发布与验证

可发布文件仅为说明、Skill 和元数据。仓库忽略本机配置、缓存、日志、下载材料和常见凭据文件；`.gitignore` 无法替代发布前检查，也不能保护已经被 Git 跟踪的秘密。

结构验证可使用 Codex 的 `skill-creator/scripts/quick_validate.py`，将 `skills/monash-moodle` 作为参数。结构验证不等于真实 Moodle 登录或每一种查询均已通过。发布前检查 Git 将提交的文件，不复制个人成绩、会话或部署输出。

## 许可与署名

采用 MIT License，见 [LICENSE](LICENSE)。上游工具和其文档由 bunizao 维护；本项目根据其命令接口整理工作流，并保留上游许可署名。个人查询经验不等同于对上游代码的修改。
