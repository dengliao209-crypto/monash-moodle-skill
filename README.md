# Monash Moodle Skill

一个独立的 Codex Skill，用自然语言查询 Monash Moodle 的 due、成绩、考试要求和考勤规定，并指导登录恢复。

**这是查询工作流，不是独立的 Moodle 客户端。**需要可用的 `moodle-cli`、Moodle MCP 或已登录浏览器；安装 Skill 不会自动完成学校认证。

An independent workflow skill for Monash Moodle deadlines, grades and exam requirements. It needs a working Moodle CLI, MCP connection or authenticated browser. It does not bundle a client or credentials. [English quick start](#english-quick-start).

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

### 从零到第一次查询

1. **已有可用Moodle MCP？**直接使用现有连接，跳到第3步；不需要为此部署Cloudflare。
2. **本机使用CLI？**先安装上游依赖。npm方式需要Node.js 22.13+（本项目检查过的CLI基线为0.10.0）；其他安装方式见[上游说明](https://github.com/bunizao/moodle-cli#install-and-sign-in-manually)。

   ```bash
   npm install -g moodle-cli
   moodle --version
   MOODLE_BASE_URL=https://learning.monash.edu moodle auth login
   MOODLE_BASE_URL=https://learning.monash.edu moodle user --json
   ```

   在学校页面完成SSO/MFA。成功标志是最后一步返回已登录账户，而不是仅存在缓存。账户输出留在本机，不贴到公开Issue。
3. **安装本Skill。**把本仓库的 `skills/monash-moodle` 文件夹放入下文说明的Codex Skill目录。已有同名目录先备份，不直接覆盖。
4. **重新打开会话，验证识别。**输入：`用 $monash-moodle 检查未来14天 due，标明时区、完成状态和来源。` 成功时应返回有来源的结果；零条也应明确查询范围，而非断言没有任何作业。
5. **遇到问题先判断是哪一层。**找不到 `moodle` 是安装/PATH问题；有缓存但读取失败可能是会话失效；MCP能读而浏览器要求登录，是两个独立会话。不要为同一个失败反复授权Security弹窗。参见[访问恢复指南](skills/monash-moodle/references/access.md)。

不需要把cookie、密码或学校数据加入仓库，也不需要为一次查询安装后台续期任务。

下载或克隆本仓库后，将 `skills/monash-moodle` 整个文件夹复制到 Codex 用户 Skill 目录，通常是 `~/.codex/skills/monash-moodle`；设置了 `CODEX_HOME` 时使用其下的 `skills/monash-moodle`。若目标目录已存在，先比较或备份，避免覆盖自定义内容。重新打开会话并确认 Skill 可见。

本仓库是可分发源文件，不会自动安装 CLI、创建 Cloudflare Worker 或复制登录状态。

在学校登录页面完成 SSO/MFA。会话有效时通常不必每次重新登录；过期或被撤销后仍需重新认证。MCP 的连接与登录恢复流程见 Skill 内的访问指南。

## 使用示例

- `用 $monash-moodle 检查未来两周 due。`
- `查一下我这学期 1001 的期末考试要求，给出来源。`
- `看看期中考试成绩；还没有公布的请单独注明。`
- `检查当前四门课有没有考勤或 participation 要求。`
- `Moodle CLI 又弹出 macOS Security，帮我判断是哪种授权。`

Skill 默认允许自动匹配相关 Monash 查询。若同时安装上游 `moodle-cli` Skill，可用 `$monash-moodle` 明确选择本流程。课程简称存在歧义时会先核对课程列表。

### 结果应该长什么样

以下完全是虚构示例，不是学校实际安排：

| 课程 | 项目 | 时间 | 状态/证据 |
|---|---|---|---|
| DEMO1000 | Quiz 5 | 11月2日23:55，标题注明 | 已开放，完成状态未知；活动关闭设置未返回 |
| DEMO2000 | Online Exercise | 10月30日23:55，活动设置 | 此页面不需上传，外部练习完成状态未知 |

正常结果应提供实际来源链接和时区。查询不完整、旧年份文件、学生未获回复的提问都应明确说明。详细流程见[查询指南](skills/monash-moodle/references/course-queries.md)。

## English quick start

1. Use an existing Moodle MCP connection, or install [upstream moodle-cli](https://github.com/bunizao/moodle-cli) and authenticate to `https://learning.monash.edu` through your university's SSO/MFA.
2. Copy `skills/monash-moodle` into your Codex user skill directory, usually `~/.codex/skills/monash-moodle`. Back up an existing folder first. If `CODEX_HOME` is configured, use its `skills` directory.
3. Open a new session and ask: `Use $monash-moodle to check deadlines in the next 14 days, including timezone, completion evidence and source links.`
4. A successful authenticated read verifies access. Cached credentials alone do not. Browser, CLI and remote MCP sessions can expire independently.

The skill checks opening versus closing events, flags unknown external-platform completion, and falls back to a targeted authenticated page read when connector output lacks assessment details. It does not start quizzes or submit work. Install/usage counts are not tracked by this repository.

For problems, open an [Issue](https://github.com/dengliao209-crypto/monash-moodle-skill/issues/new/choose) using a template. Share only redacted errors and versions, never cookies, tokens, grades or private course files.

## 发布与验证

可发布文件仅为公开说明、Skill、元数据和虚构评估用例。仓库忽略本机配置、缓存、日志、下载材料和常见凭据文件；`.gitignore` 无法替代发布前检查，也不能保护已经被 Git 跟踪的秘密。

结构验证可使用 Codex 的 `skill-creator/scripts/quick_validate.py`，将 `skills/monash-moodle` 作为参数。结构验证不等于真实 Moodle 登录或每一种查询均已通过。发布前检查 Git 将提交的文件，不复制个人成绩、会话或部署输出。

行为回归用例见[评估案例](tests/scenarios.md)，覆盖漏掉的开放测验、外部练习和正文缺失等情况；用例不需要真实账号。发布记录见[CHANGELOG](CHANGELOG.md)。当前文档基线为CLI 0.10.0，并非声称兼容所有后续版本。

## 许可与署名

采用 MIT License，见 [LICENSE](LICENSE)。上游工具和其文档由 bunizao 维护；本项目根据其命令接口整理工作流，并保留上游许可署名。个人查询经验不等同于对上游代码的修改。
