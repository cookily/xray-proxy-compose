# xray-proxy

> 基于 Docker 的 xray 代理部署方案，换服务器时一行命令即可迁移。

**版本：26.5.9**（xray 官方版本）

---

## 源码来源

本项目的 Docker 镜像使用 [XTLS/Xray-core](https://github.com/XTLS/Xray-core) 发布的 xray 二进制构建，采用 MIT 许可证。

**上游项目：**
- [XTLS/Xray-core](https://github.com/XTLS/Xray-core) - xray 核心（MIT 许可证）

---

## 快速部署（3 步）

```bash
# 1. 复制仓库
git clone https://github.com/yourname/xray-proxy-compose.git
cd xray-proxy-compose

# 2. 填入节点信息
cp config.json.example config.json
vim config.json

# 3. 启动
docker compose up -d
```

---

## 前置要求

- Docker 20.10+
- Docker Compose v2+

```bash
# 安装 Docker（Ubuntu）
curl -fsSL https://get.docker.com | sh

# 安装 Docker Compose
apt update && apt install -y docker-compose
```

---

## 文件说明

```
xray-proxy-compose/
├── docker-compose.yml      # 容器编排配置
├── config.json.example     # 配置模板（复制后填入节点信息）
├── config.json            # 你的节点配置（不传 GitHub）
├── README.md              # 本文件
└── .gitignore             # 忽略 config.json
```

---

## 配置说明

从你的 v2rayN 客户端导出节点配置，填入 `config.json`：

| 字段 | 说明 | 示例 |
|------|------|------|
| `outbounds[0].settings.vnext[0].address` | 服务器地址 | `YOUR_SERVER_ADDRESS` |
| `outbounds[0].settings.vnext[0].port` | 端口 | `443` |
| `outbounds[0].settings.vnext[0].users[0].id` | UUID | `YOUR_UUID` |
| `streamSettings.realitySettings.publicKey` | Reality 公钥 | `YOUR_PUBLIC_KEY` |
| `streamSettings.realitySettings.shortId` | Reality ShortId | `YOUR_SHORT_ID` |

---

## 管理命令

```bash
docker compose up -d        # 启动
docker compose down         # 停止
docker compose restart      # 重启
docker compose logs -f     # 查看日志
docker compose pull         # 更新镜像
docker compose ps          # 查看状态
```

---

## 验证

```bash
curl -s -x http://127.0.0.1:10808 -o /dev/null -w "%{http_code}\n" https://github.com
# 返回 200 即成功
```

---

## 设置系统代理（可选）

```bash
# Git 代理
git config --global http.proxy http://127.0.0.1:10808
git config --global https.proxy http://127.0.0.1:10808

# npm 代理
npm config set proxy http://127.0.0.1:10808
npm config set https-proxy http://127.0.0.1:10808

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

## 故障排查

```bash
# 容器没启动
docker compose logs --tail 20

# 端口没监听
ss -tlnp | grep -E "10808|10809"

# 代理不通
curl -v --connect-timeout 10 -x http://127.0.0.1:10808 https://github.com
```

---

## 镜像说明

Docker 镜像基于 `alpine:3.19`，包含：
- xray 26.5.9
- 体积小（~50MB）

镜像地址：`yourname/xray-proxy:26.5.9`

---

## 免责声明

1. **用途限制**：本项目仅供学习技术交流使用，请勿用于任何违法违规用途。在某些地区或场景下使用本项目可能违反当地法律法规，使用者需自行确认并承担全部责任。

2. **节点责任**：本项目不提供任何代理节点，所有节点配置需由用户自行准备。

3. **无担保**：使用本项目即表示同意自行承担全部风险，作者不对任何直接或间接损失负责，包括但不限于数据丢失、业务中断等。

4. **商标声明**：xray 和 XTLS 是 [XTLS/Xray-core](https://github.com/XTLS/Xray-core) 的注册商标，本项目与此项目无关联。

5. **许可证**：xray-core 采用 [MIT 许可证](https://github.com/XTLS/Xray-core/blob/main/LICENSE)。

---

*文档生成时间：2026-05-13*
