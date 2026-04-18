# knotodo

`knotodo` 是一个轻量的 calendar / todo / kanban 原型，适合先验证个人效率工具的交互闭环。后端使用 FastAPI，前端使用 React + Vite，数据默认持久化到本地 JSON 文件。

## 功能

- 月历总览与日视图
- Todo、时间块、截止事项
- 模板与搜索
- 轻量看板（board / lane / card）
- 本地 JSON 持久化与备份

## 技术栈

- 后端：FastAPI、Pydantic、filelock
- 前端：React 19、Vite
- 运行时：Python 3.13、Node.js 20
- 部署：Docker Compose，可选 Nginx 反向代理

## 本地开发

```bash
cp .env.example .env
uv sync
npm --prefix frontend ci
uv run main.py
```

默认会启动：

- 前端开发服务：`http://127.0.0.1:4173`
- 后端 API：`http://127.0.0.1:8081`
- 健康检查：`http://127.0.0.1:8081/api/health`

说明：

- 本地开发默认由 `main.py` 同时拉起 FastAPI 和 Vite。
- 生产部署默认关闭 Vite，并由 FastAPI 直接托管 `frontend/dist`。

## Docker 部署

```bash
cp .env.example .env
docker compose up -d --build
```

默认入口：

- 应用：`http://127.0.0.1:4173`
- 健康检查：`http://127.0.0.1:8081/api/health`

## 反向代理部署

如果希望由 Nginx 接管 `80/443`，可以使用高位端口的宿主机绑定方式：

```bash
cp .env.domain.example .env
docker compose -f docker-compose.domain.yml up -d --build
```

仓库中附带了通用示例配置：[deploy/nginx/knotodo.example.conf](deploy/nginx/knotodo.example.conf)。

这台服务器如果已经在用私有 Nginx 配置，可以继续保留本地文件，不需要因为开源整理而改线上域名映射。

默认映射：

- `todo.example.com -> 127.0.0.1:18082`
- `127.0.0.1:18083` 保留给后端直连排障或健康检查

## 环境变量

| 变量 | 默认值 | 说明 |
| --- | --- | --- |
| `KNOTODO_FRONTEND_PORT` | `4173` | 宿主机访问应用时使用的入口端口 |
| `KNOTODO_BACKEND_PORT` | `8081` | FastAPI 实际监听端口 |
| `KNOTODO_LOG_LEVEL` | `INFO` | 日志级别 |
| `KNOTODO_SERVE_BUILT_FRONTEND` | `0` | 是否由 FastAPI 托管 `frontend/dist` |
| `KNOTODO_DISABLE_FRONTEND` | `0` | 是否禁用本地 Vite 开发服务 |
| `KNOTODO_DB_FILE` | 自动推导 | JSON 数据文件路径 |
| `KNOTODO_LOCK_FILE` | 自动推导 | 文件锁路径 |
| `KNOTODO_DB_BACKUP_FILE` | 自动推导 | 备份文件路径 |
| `KNOTODO_LOCK_TIMEOUT` | `10` | 文件锁超时时间（秒） |

容器部署会强制启用内置静态资源托管，因此 `.env.example` 更偏向本地开发默认值。

## 项目结构

```text
knotodo/
├── api/                    # FastAPI 路由、请求模型、错误映射
├── core/                   # 日历 / todo / 看板核心逻辑与存储
├── frontend/               # React + Vite 前端
├── utils/                  # 运行辅助逻辑
├── deploy/nginx/           # 反向代理示例
├── docker-compose.yml      # 通用容器部署
├── docker-compose.domain.yml
├── main.py                 # 应用入口
└── local_data/             # 运行期生成，不纳入版本控制
```

## 版本控制约定

建议提交：

- 源代码、部署配置、示例环境文件
- `uv.lock`
- `frontend/package-lock.json`

不要提交：

- `.env`
- `local_data/` 下的运行数据
- `frontend/node_modules/`、`frontend/dist/`
- 服务器私有部署文件（例如本机域名专用 Nginx 配置）
- `.venv/`、`__pycache__/`、编辑器配置、日志文件

贡献流程见 [CONTRIBUTING.md](CONTRIBUTING.md)。
