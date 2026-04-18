# Nginx 反向代理示例

如果你使用 `docker-compose.domain.yml`，推荐让 KnoTodo 只监听本机高位端口，再由 Nginx 接管 `80/443`。

```bash
cp .env.domain.example .env
docker compose -f docker-compose.domain.yml up -d --build
```

默认映射：

- `todo.example.com -> 127.0.0.1:18082`
- `127.0.0.1:18083` 用于后端直连排障或健康检查

随后：

1. 根据实际域名修改 [knotodo.example.conf](knotodo.example.conf) 中的 `server_name`。
2. 把配置放到 Nginx 站点目录。
3. 重载 Nginx。

如果当前服务器已经有正在使用的私有配置文件，可以继续保留本地版本，不需要纳入开源仓库。
