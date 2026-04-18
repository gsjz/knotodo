# Contributing

## 本地开发

```bash
cp .env.example .env
uv sync
npm --prefix frontend ci
uv run main.py
```

默认本地模式会启动：

- FastAPI：`127.0.0.1:8081`
- Vite：`127.0.0.1:4173`

## 提交边界

单个 commit 只做一类事情，不要把以下内容混在一起：

- 功能改动
- 重构
- 文档改动
- 部署或构建配置
- 示例数据调整

如果一次改动跨越多个层面，优先拆成多个小 commit，让每个 commit 都能独立说明“为什么改”和“改了什么”。

## Commit Message

推荐格式：

```text
type(scope): summary
```

常用类型：

- `feat`：新功能
- `fix`：缺陷修复
- `refactor`：不改变行为的重构
- `docs`：文档
- `build`：构建、容器、依赖、CI
- `chore`：仓库整理、忽略规则、示例数据等杂项

示例：

- `feat(kanban): support lane archiving`
- `fix(api): return 404 for missing board`
- `refactor(storage): split calendar queries and mutations`
- `docs(readme): document local and docker workflows`
- `build(docker): exclude local state from image context`
- `chore(repo): ignore local env and runtime artifacts`

## 推荐提交流程

如果你在做类似“发布前整理”这类改动，建议按下面顺序提交：

1. 仓库边界
   `chore(repo): ignore local env, caches, and runtime data`
2. 构建与部署
   `build(docker): exclude local files from image context`
3. 文档主入口
   `docs(readme): rewrite setup and deployment guide`
4. 补充协作说明
   `docs(contrib): add contribution and commit guidelines`
5. 示例与样例清理
   `chore(seed): anonymize demo seed data`

这样拆分后，回滚和审阅都会更容易。

## Pull Request Checklist

- 变更范围清晰，commit 没有混杂无关修改
- 新增环境变量时同步更新 `.env.example` 或相关文档
- 新增部署方式时同步更新 `README.md`
- 不提交 `.env`、`local_data/`、构建产物和依赖目录
- 不提交服务器私有域名、证书路径或机器专用 Nginx 配置
