# Nginx
***
## 反向代理
- 配置示例：
  nginx.conf
  ```
  server {
      listen 80;              # 监听端口
      server_name localhost;  # 服务器名称
  
      location /api/ {
          proxy_pass http://localhost:8080/admin/;  # 转发请求到内部服务器
      }
  }
  ```
- 说明：
  listen：Nginx 监听的端口
  server_name：服务器域名或 IP
  location /api/：匹配以/api/开头的 URL 路径
  proxy_pass：将请求转发到指定 URL（注意末尾/的影响）
  请求 URL：http://localhost/api/users
  实际转发：http://localhost:8080/admin/users
***
## 负载均衡
- 配置实例：
  nginx.conf
  ```
  # 定义后端服务器组
  upstream webservers {
      server 192.168.100.128:8080;
      server 192.168.100.129:8080;
  }

  server {
      listen 80;
      server_name localhost;
  
     location /api/ {
          proxy_pass http://webservers/admin/;  # 使用负载均衡组
      }
  }
  ```
- 说明
  upstream：定义后端服务器组名称（如webservers）
  默认负载均衡算法：轮询（Round Robin）
  自动健康检查：自动跳过无响应的服务器
- 负载均衡策略：
  ip_hash：基于客户端 IP 分配固定服务器（解决 Session 问题）
  least_conn：分配给连接数最少的服务器
  权重配置：``` server 192.168.100.128:8080 weight=2;  # 处理2倍请求 ```

