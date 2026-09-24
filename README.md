# DeepSeek Harness 项目执行规则

本仓库提供一份可放入项目根目录的 [`AGENTS.md`](./AGENTS.md)，用于约束 DeepSeek Harness（`dsh`）中的 agent 围绕用户当前目标持续实施、验证和收尾。文件内还包含按需建立 `GOAL.md`、`DONE.md` 的模板。

## 部署到你的项目

### 1. 准备 DeepSeek Harness

安装 [Node.js](https://nodejs.org/)。DeepSeek Harness 官方提供以下启动方式：

```bash
npx @deepseek-ai/dsh web
```

模型需要在 Web 界面的 **Settings → Models** 中配置；具体密钥和模型按你的服务商设置。以下步骤先把规则文件放入目标项目，再从该项目目录启动 `dsh`。

### 2. 将规则放入目标项目根目录

在**目标项目的上一级目录**执行以下示例命令。将 `your-project` 换成你的实际项目目录名：

```bash
git clone https://github.com/3794708705/deepseek-harness-agent-rules.git

if [ -e ./your-project/AGENTS.md ]; then
  printf '目标项目已有 AGENTS.md，请阅读两份文件并手动合并适用规则。\n'
else
  cp ./deepseek-harness-agent-rules/AGENTS.md ./your-project/AGENTS.md
fi

cd ./your-project
npx @deepseek-ai/dsh web
```

如果目标项目已经有 `AGENTS.md`，请将本仓库中的适用内容合并进去，保留目标项目原有要求。部署完成后，应能在**目标项目根目录**看到 `AGENTS.md`；仅克隆本仓库不会让另一个项目自动加载这份规则。

### 3. 在 Web 界面启用并检查

1. 打开启动命令给出的地址；本地默认是 `http://127.0.0.1:3080`。
2. 在 **Settings → Models** 配置可用模型。
3. 点击 **Choose workspace**，添加并选中放有 `AGENTS.md` 的**目标项目目录**，然后开始新会话。
4. 用下面的启动指令替换方括号里的目标，发送给 agent：

> 我的唯一目标是：[具体交付结果]。读取项目 AGENTS.md，检查现状并填写 GOAL.md。在现有授权范围内持续实施、验证和修复；如果目标需要跨轮执行且原生 goal 能力可用，沿用或创建同一个完成目标。不要为内部步骤另建项目。达到验收后先保存成果与完成记录，再结束原生 goal 并停止。只有真实阻塞或必要授权缺失时才提出具体问题。

首次使用可先让 agent 读取当前项目的 `AGENTS.md` 并指出其中的「核心约定」，核对会话选中的工作区是否正确。`GOAL.md` 和 `DONE.md` 按文件中的规则在需要时建立；把模板仓库单独打开为工作区，不会使规则作用于其他项目。

## 加载不到规则时

- 确认会话选择的是包含目标项目 `AGENTS.md` 的目录。官方加载器默认以 `.git` 标记项目根目录，读取从项目根目录到会话工作目录的适用文件。
- 官方基础组合默认启用 `dsh-agent-instructions`；自定义组合可能关闭该插件。检查当前组合是否启用了它，或让 agent 明确读取文件内容。
- 默认候选还包括 `CLAUDE.md`；两份文件内容不同时可能同时加载。若有冲突，请检查并统一适用规则。
- 外部修改并非持续监听；在新会话中重新检查，或用当前可用的文件读取工具触发刷新。超过配置的上下文预算时，留意规则遗漏或截断提示。

## 依据

- [DeepSeek Harness 官方 README：npm 启动命令](https://github.com/deepseek-ai/deepseek-harness#run-from-npm)
- [官方 Web UI 使用指南：模型和工作区设置](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/guide/index.md)
- [工作区指令加载器：项目根目录、候选文件与刷新机制](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/context/agent-instructions/README.md)

本仓库只包含工作规则。DeepSeek Harness 本身的安装、账号与模型配置以其官方文档和你当前使用的版本为准。
