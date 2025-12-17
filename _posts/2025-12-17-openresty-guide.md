---
layout: post
title: "OpenResty 使用总结"
date: 2025-12-17
categories: [OpenResty, Nginx, Lua]
---

# OpenResty 使用总结

## 目录

- [OpenResty 简介](#openresty-简介)
- [安装部署](#安装部署)
- [核心概念](#核心概念)
- [常用 API](#常用api)
- [实战应用](#实战应用)
- [性能优化](#性能优化)
- [最佳实践](#最佳实践)

## OpenResty 简介

OpenResty 是一个基于 Nginx 与 Lua 的高性能 Web 平台，它将 Nginx 核心、LuaJIT、许多精心编写的 Lua 库以及第三方 Nginx 模块整合在一起。

### 核心特性

- **高性能**: 基于 Nginx 和 LuaJIT，性能接近原生 C
- **动态性**: 使用 Lua 实现业务逻辑，无需重新编译
- **非阻塞**: 完全非阻塞的网络 I/O
- **丰富的 API**: 提供大量的 Lua API 操作 HTTP 请求

## 安装部署

### Linux 安装

```bash
# Ubuntu/Debian
sudo apt-get install -y openresty

# CentOS/RHEL
sudo yum install -y openresty

# 源码编译
wget https://openresty.org/download/openresty-1.21.4.1.tar.gz
tar -zxvf openresty-1.21.4.1.tar.gz
cd openresty-1.21.4.1
./configure --prefix=/usr/local/openresty
make && sudo make install
```

### 基本配置

```nginx
# nginx.conf
worker_processes auto;
error_log logs/error.log;

events {
    worker_connections 1024;
}

http {
    lua_package_path "/usr/local/openresty/lualib/?.lua;;";
    lua_package_cpath "/usr/local/openresty/lualib/?.so;;";

    server {
        listen 8080;

        location /hello {
            default_type text/html;
            content_by_lua_block {
                ngx.say("<p>Hello, OpenResty!</p>")
            }
        }
    }
}
```

## 核心概念

### 执行阶段

OpenResty 将 Nginx 请求处理分为多个阶段，每个阶段都可以用 Lua 处理：

```nginx
init_by_lua_block       # Nginx master 进程加载配置时执行
init_worker_by_lua_block # 每个 worker 进程启动时执行
set_by_lua_block        # 设置变量
rewrite_by_lua_block    # rewrite 阶段
access_by_lua_block     # 访问控制阶段
content_by_lua_block    # 内容生成阶段
header_filter_by_lua_block  # 响应头过滤
body_filter_by_lua_block    # 响应体过滤
log_by_lua_block        # 日志记录阶段
```

### 示例：各阶段使用

```nginx
server {
    listen 8080;

    location /test {
        # 重写阶段
        rewrite_by_lua_block {
            ngx.log(ngx.ERR, "rewrite phase")
        }

        # 访问控制
        access_by_lua_block {
            local ip = ngx.var.remote_addr
            if ip == "127.0.0.1" then
                return
            end
            ngx.exit(ngx.HTTP_FORBIDDEN)
        }

        # 内容生成
        content_by_lua_block {
            ngx.say("Access granted!")
        }
    }
}
```

## 常用 API

### 请求相关

```lua
-- 获取请求方法
local method = ngx.var.request_method

-- 获取URI和参数
local uri = ngx.var.uri
local args = ngx.req.get_uri_args()

-- 获取请求头
local headers = ngx.req.get_headers()
local user_agent = headers["User-Agent"]

-- 获取请求体
ngx.req.read_body()
local body = ngx.req.get_body_data()

-- 获取POST参数
local post_args = ngx.req.get_post_args()
```

### 响应相关

```lua
-- 设置响应状态码
ngx.status = ngx.HTTP_OK

-- 设置响应头
ngx.header["Content-Type"] = "application/json"
ngx.header["X-Custom-Header"] = "value"

-- 输出响应
ngx.say("Hello")  -- 输出并添加换行
ngx.print("World") -- 输出不换行

-- 输出JSON
local cjson = require "cjson"
ngx.say(cjson.encode({status = "ok", message = "success"}))

-- 重定向
ngx.redirect("/new-url", ngx.HTTP_MOVED_PERMANENTLY)

-- 退出请求
ngx.exit(ngx.HTTP_NOT_FOUND)
```

### HTTP 客户端

```lua
local http = require "resty.http"
local httpc = http.new()

-- 发起HTTP请求
local res, err = httpc:request_uri("http://api.example.com/data", {
    method = "POST",
    body = '{"key":"value"}',
    headers = {
        ["Content-Type"] = "application/json",
    },
    ssl_verify = false
})

if not res then
    ngx.log(ngx.ERR, "request failed: ", err)
    return
end

ngx.say("Status: ", res.status)
ngx.say("Body: ", res.body)
```

### 共享内存字典

```nginx
# nginx.conf
http {
    lua_shared_dict my_cache 10m;

    server {
        location /cache {
            content_by_lua_block {
                local cache = ngx.shared.my_cache

                -- 设置值
                cache:set("key", "value", 60)  -- 60秒过期

                -- 获取值
                local val = cache:get("key")

                -- 增量操作
                cache:incr("counter", 1, 0)

                ngx.say("Cached value: ", val)
            }
        }
    }
}
```

### Redis 集成

```lua
local redis = require "resty.redis"
local red = redis:new()

red:set_timeouts(1000, 1000, 1000) -- connect, send, read timeout

-- 连接
local ok, err = red:connect("127.0.0.1", 6379)
if not ok then
    ngx.log(ngx.ERR, "failed to connect: ", err)
    return
end

-- 操作
red:set("key", "value")
local res, err = red:get("key")

-- 连接池
local ok, err = red:set_keepalive(10000, 100)
```

### MySQL 集成

```lua
local mysql = require "resty.mysql"
local db, err = mysql:new()

db:set_timeout(1000)

-- 连接
local ok, err = db:connect({
    host = "127.0.0.1",
    port = 3306,
    database = "test",
    user = "root",
    password = "password"
})

-- 查询
local res, err = db:query("SELECT * FROM users WHERE id = 1")
if not res then
    ngx.log(ngx.ERR, "query failed: ", err)
    return
end

-- 连接池
local ok, err = db:set_keepalive(10000, 100)
```

## 实战应用

### 1. API 网关

```nginx
location /api/ {
    access_by_lua_block {
        -- 验证 API Key
        local api_key = ngx.req.get_headers()["X-API-Key"]
        if not api_key or api_key ~= "secret-key" then
            ngx.status = ngx.HTTP_UNAUTHORIZED
            ngx.say('{"error":"Unauthorized"}')
            ngx.exit(ngx.HTTP_UNAUTHORIZED)
        }
    }

    content_by_lua_block {
        -- 路由到后端服务
        local http = require "resty.http"
        local httpc = http.new()

        local res, err = httpc:request_uri("http://backend:8080" .. ngx.var.uri, {
            method = ngx.var.request_method,
            body = ngx.req.get_body_data(),
            headers = ngx.req.get_headers()
        })

        if not res then
            ngx.status = ngx.HTTP_SERVICE_UNAVAILABLE
            ngx.say('{"error":"Backend unavailable"}')
            return
        end

        ngx.status = res.status
        ngx.say(res.body)
    }
}
```

### 2. 限流

```nginx
http {
    lua_shared_dict limit_count 10m;

    server {
        location /limited {
            access_by_lua_block {
                local limit = require "resty.limit.count"
                local lim, err = limit.new("limit_count", 10, 60) -- 10次/分钟

                if not lim then
                    ngx.log(ngx.ERR, "failed to create limit: ", err)
                    return
                end

                local key = ngx.var.binary_remote_addr
                local delay, err = lim:incoming(key, true)

                if not delay then
                    if err == "rejected" then
                        ngx.header["X-RateLimit-Limit"] = "10"
                        ngx.header["X-RateLimit-Remaining"] = "0"
                        return ngx.exit(429)
                    end
                    ngx.log(ngx.ERR, "failed to limit: ", err)
                    return
                end
            }

            content_by_lua_block {
                ngx.say("Request allowed")
            }
        }
    }
}
```

### 3. 缓存代理

```nginx
location /cache-proxy {
    content_by_lua_block {
        local cache = ngx.shared.my_cache
        local key = ngx.var.uri

        -- 尝试从缓存获取
        local cached = cache:get(key)
        if cached then
            ngx.say(cached)
            return
        end

        -- 缓存未命中，请求后端
        local http = require "resty.http"
        local httpc = http.new()
        local res, err = httpc:request_uri("http://backend" .. ngx.var.uri)

        if not res then
            ngx.log(ngx.ERR, "backend error: ", err)
            ngx.exit(ngx.HTTP_SERVICE_UNAVAILABLE)
            return
        end

        -- 缓存结果
        cache:set(key, res.body, 300) -- 缓存5分钟
        ngx.say(res.body)
    }
}
```

### 4. JWT 验证

```lua
local jwt = require "resty.jwt"
local secret = "your-secret-key"

-- 验证 JWT Token
local token = ngx.req.get_headers()["Authorization"]
if not token then
    ngx.exit(ngx.HTTP_UNAUTHORIZED)
end

token = string.gsub(token, "Bearer ", "")
local jwt_obj = jwt:verify(secret, token)

if not jwt_obj.verified then
    ngx.log(ngx.ERR, "JWT verification failed: ", jwt_obj.reason)
    ngx.exit(ngx.HTTP_UNAUTHORIZED)
end

-- Token 有效，继续处理
ngx.say("User ID: ", jwt_obj.payload.user_id)
```

## 性能优化

### 1. 使用连接池

```lua
-- Redis 连接池
local red = redis:new()
red:connect("127.0.0.1", 6379)
-- ... 操作 ...
red:set_keepalive(10000, 100)  -- 10秒超时，100个连接池大小

-- MySQL 连接池
local db = mysql:new()
db:connect({...})
-- ... 操作 ...
db:set_keepalive(10000, 100)
```

### 2. 使用共享内存

```nginx
# 避免每次请求都从外部服务获取数据
lua_shared_dict config_cache 1m;
lua_shared_dict data_cache 10m;
```

### 3. 启用 LuaJIT

```nginx
# OpenResty 默认使用 LuaJIT，确保编译时启用
./configure --with-luajit
```

### 4. 合理使用缓存

```lua
-- 本地缓存（单worker）
local lrucache = require "resty.lrucache"
local cache, err = lrucache.new(200)  -- 缓存200个条目

-- 共享缓存（所有worker）
local cache = ngx.shared.my_cache
```

## 最佳实践

### 1. 错误处理

```lua
-- 始终检查错误
local res, err = some_function()
if not res then
    ngx.log(ngx.ERR, "error: ", err)
    return ngx.exit(ngx.HTTP_INTERNAL_SERVER_ERROR)
end
```

### 2. 避免阻塞操作

```lua
-- 使用非阻塞的 cosocket API
-- 好的做法
local sock = ngx.socket.tcp()
sock:settimeout(1000)
sock:connect("127.0.0.1", 6379)

-- 避免阻塞的 Lua 标准库
-- 不要使用 io.*, os.execute 等
```

### 3. 模块化代码

```lua
-- lib/utils.lua
local _M = {}

function _M.validate_request()
    -- 验证逻辑
end

return _M

-- nginx.conf
content_by_lua_block {
    local utils = require "lib.utils"
    utils.validate_request()
}
```

### 4. 日志记录

```lua
-- 使用不同的日志级别
ngx.log(ngx.DEBUG, "debug info")
ngx.log(ngx.INFO, "info message")
ngx.log(ngx.WARN, "warning")
ngx.log(ngx.ERR, "error occurred")
```

### 5. 安全性

```lua
-- 输入验证
local args = ngx.req.get_uri_args()
local id = tonumber(args.id)
if not id or id <= 0 then
    return ngx.exit(ngx.HTTP_BAD_REQUEST)
end

-- 防止 SQL 注入（使用参数化查询）
local res = db:query("SELECT * FROM users WHERE id = ?", {id})

-- 限制请求大小
client_max_body_size 1m;
```

## 总结

OpenResty 是一个强大的 Web 平台，它结合了 Nginx 的高性能和 Lua 的灵活性。主要优势：

- **高性能**: 适合高并发场景
- **灵活性**: 使用 Lua 快速实现业务逻辑
- **丰富的生态**: 大量可用的 Lua 库
- **成熟稳定**: 被广泛应用于生产环境

适用场景：

- API 网关
- 反向代理
- Web 应用防火墙 (WAF)
- 缓存服务
- 动态路由
- 服务编排

## 参考资源

- [OpenResty 官方文档](https://openresty.org/)
- [OpenResty 最佳实践](https://github.com/moonbingbing/openresty-best-practices)
- [lua-nginx-module 文档](https://github.com/openresty/lua-nginx-module)
- [awesome-resty](https://github.com/bungle/awesome-resty)
