# publicPrivacy

诗词闯关记（com.poemquest.poetry）的公开隐私政策页，通过 GitHub Pages 对外访问。

## 访问地址

开启 Pages 后访问：

- 根地址：https://shine-zyy.github.io/publicPrivacy/
- 政策页：https://shine-zyy.github.io/publicPrivacy/privacy-policy.html

（填写应用市场资料时建议直接用带 `privacy-policy.html` 的完整地址。）

## 发布前必改（3 处占位）

`privacy-policy.html` 里有 3 处需要替换，搜索 `【待填写` 即可定位：

| 占位 | 位置 | 替换为 |
|---|---|---|
| `【待填写：开发者姓名或主体名称】` | 头部「开发者」+ 第九章（2 处同名，一起替换） | 你的姓名或公司主体名称 |
| `【待填写：联系邮箱】` | 第九章 | 常用邮箱 |
| `【待填写：承诺回复天数，如 15】` | 第九章 | 工作日天数 |

一键替换示例（把中文内容换成你自己的）：

```bash
cd publicPrivacy
sed -i '' 's/【待填写：开发者姓名或主体名称】/张某某/g' privacy-policy.html
sed -i '' 's/【待填写：联系邮箱】/you@example.com/g' privacy-policy.html
sed -i '' 's/【待填写：承诺回复天数，如 15】/15/g' privacy-policy.html
```

## 开启 GitHub Pages

1. 打开 https://github.com/shine-zyy/publicPrivacy
2. `Settings` → 左侧 `Pages`
3. `Build and deployment` → **Source** 选 `Deploy from a branch`
4. **Branch** 选 `main`、目录选 `/ (root)` → `Save`
5. 等 1–3 分钟，刷新页面顶部会出现访问地址（绿色提示 “Your site is live at …”）

之后每次改完 `privacy-policy.html` 并 push 到 main，约 1 分钟后线上自动更新。

## 本地预览

```bash
cd publicPrivacy
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000/privacy-policy.html
```

## 常见问题：git push 没反应 / 卡住

如果在某些终端里 `git push` 长时间无输出，多半是环境里的 HTTP 代理拦了 `github.com:443`
（表现为 `CONNECT tunnel failed, response 502`）。两种解法：

1. **改用 SSH**（推荐，本机已有密钥 `~/.ssh/id_ed25519.pub`）：
   ```bash
   git remote set-url origin git@github.com:shine-zyy/publicPrivacy.git
   git push -u origin main
   ```
   若提示 `Permission denied (publickey)`，把公钥添加到
   https://github.com/settings/ssh/new 再试。

2. **继续用 HTTPS**：清掉代理变量后再推（会要求输入用户名 + Personal Access Token）：
   ```bash
   env -u http_proxy -u https_proxy -u HTTP_PROXY -u HTTPS_PROXY git push -u origin main
   ```
