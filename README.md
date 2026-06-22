# Chief Worker Flow

`chief-worker-flow` 是一个 Codex 工作流 skill，用于把复杂任务拆成“主会话 chief + 一次性 worker”的协作模式。

## 适用场景

- 图片、视频、音频等多模态任务需要隔离处理。
- 大文件、批处理、资料检索、provider 调用或多候选测试会拖慢主会话。
- 一个任务可以拆成多个独立分支并行执行。
- 需要主会话只保留路径、链接、决策和简短结论，而不加载大量原始内容。

## 核心规则

- 主会话负责理解用户意图、拆分任务、验收结果和最终汇报。
- worker 负责有边界地执行、返回证据、然后停止。
- worker 完成、阻塞、超时弃用或任务切换后，chief 必须主动关闭 worker。
- worker 自称 `closed: yes` 不等于工具层已经关闭；最终回复前必须做 lifecycle audit。

## 安装

将本仓库复制到 Codex skills 目录，例如：

```bash
mkdir -p ~/.codex/skills
cp -R chief-worker-flow ~/.codex/skills/chief-worker-flow
```

然后在 Codex 中使用 `$chief-worker-flow` 或让任务自然触发该 skill。
