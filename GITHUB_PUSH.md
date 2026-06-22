# GitHub 推送指南（给下一个 AI 看）

## 项目信息

- **仓库**：https://github.com/Zy-1412/astrbot-repeat-plus
- **本地路径**：`/workspace/out341`
- **核心文件**：`main.py` / `metadata.yaml` / `_conf_schema.json` / `README.md`

## 推送流程

```bash
cd /workspace/out341

# 1. 设置 Git 身份（如果还没设）
git config user.name "AstrBot Plugin"
git config user.email "astrbot@example.com"

# 2. 配置 remote（需要用户提供 GitHub Personal Access Token）
# 格式：https://{TOKEN}@github.com/Zy-1412/astrbot-repeat-plus.git
git remote set-url origin https://{TOKEN}@github.com/Zy-1412/astrbot-repeat-plus.git

# 3. 添加文件并提交
git add main.py metadata.yaml _conf_schema.json README.md
git commit -m "描述你的改动"

# 4. 推送
git push origin master
```

## 获取 Token

让用户去 https://github.com/settings/tokens 创建一个 **Personal Access Token (classic)**，勾选 `repo` 权限，然后把 token 给你。

Token 格式类似：`ghp_xxxxxxxxxxxxxxxxxxxx`

拿到 token 后，替换上面第 2 步中的 `{TOKEN}`。

## 版本号升级规则

改版本号时，以下 5 个位置必须同步更新：

| 文件 | 位置 | 示例 |
|------|------|------|
| `main.py` 第 1 行 | docstring | `v1.2.3 — ...` |
| `main.py` ~第 308 行 | 加载日志 | `插件已加载 v1.2.3` |
| `main.py` ~第 2078 行 | 帮助指令 | `复读插件 v1.2.3 指令帮助` |
| `main.py` ~第 2090 行 | 帮助页脚 | `v1.2.3 正式版：...` |
| `metadata.yaml` | version 字段 | `version: v1.2.3` |

> 行号可能因代码改动而偏移，用 `grep v1\.2\.` 搜索确认。

## 验证语法

```bash
cd /workspace/out341 && python3 -c "import py_compile; py_compile.compile('main.py', doraise=True); print('OK')"
```

## 常见问题

### 推送提示 "Author identity unknown"
```bash
git config user.name "AstrBot Plugin"
git config user.email "astrbot@example.com"
```

### 推送提示认证失败
检查 remote 地址是否包含正确的 token：
```bash
git remote -v
# 正确：https://ghp_xxx@github.com/Zy-1412/astrbot-repeat-plus.git
# 错误：https://github.com/Zy-1412/Repeat-plus.git
```

### 仓库名改了
旧名 `Repeat-plus`，新名 `astrbot-repeat-plus`。旧地址会自动重定向，但建议用新地址。

### 推送被 secret scanning 拦截
不要把 token 明文写在代码文件里。token 只放在 `git remote set-url` 命令中，不会被提交到仓库。