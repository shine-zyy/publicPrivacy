# publicPrivacy

开发者 张艳艳 各应用的公开隐私政策页，通过 GitHub Pages 对外访问。

## 目录结构

```
index.html                        政策索引（列出所有页面）
shared/                           ★ 统一政策页：同一开发者系列应用共用
  privacy-policy.html             隐私政策
  child-privacy.html              儿童隐私政策
  child-agreement.html            儿童隐私协议
privacy-policy.html               诗词闯关记（历史单应用页，保持可用）
child-privacy.html
privacy-policy-literacy.html      识字闯关记
child-privacy-literacy.html
privacy-policy-moodmonster.html   心情小怪兽
child-privacy-moodmonster.html
```

## ★ 统一政策页（新应用优先用这套）

一份 HTML 同时服务多款应用：**应用名 / 包名从 URL 参数读**，页面自己填进去。
所以同类应用只需复用同一组链接、换参数，不用再写一份政策。

三个固定链接（把 `APP` / `PKG` 换成实际值并做 URL 编码）：

```
https://shine-zyy.github.io/publicPrivacy/shared/privacy-policy.html?app=APP&pkg=PKG
https://shine-zyy.github.io/publicPrivacy/shared/child-privacy.html?app=APP&pkg=PKG
https://shine-zyy.github.io/publicPrivacy/shared/child-agreement.html?app=APP&pkg=PKG
```

例（看谁反应快）：

```
https://shine-zyy.github.io/publicPrivacy/shared/privacy-policy.html?app=%E7%9C%8B%E8%B0%81%E5%8F%8D%E5%BA%94%E5%BF%AB&pkg=com.runfast.app
```

ArkTS 里生成（`AppCopy.ets` 已经这么写）：

```ts
const URL_BASE = 'https://shine-zyy.github.io/publicPrivacy/shared';
const URL_QUERY = `?app=${encodeURIComponent(APP_NAME)}&pkg=${encodeURIComponent(PKG)}`;
```

**注意**：

- 参数一定要带全。漏了 `app` 页面就显示兜底的「本应用」，审核会认为
  「政策页里的应用名与商店资料不一致」而驳回。
- 参数只带三个 ASCII/编码后的值，别在链接里塞中文以外的私货。
- 统一页里写的是「不收集任何个人信息、未申请任何权限」这一套口径。
  **如果某个应用的实际行为不一样（申请了权限、有联网、有第三方 SDK），
  就不要用它**，必须为该应用单独出一份政策页（见下面「单应用页」）。
- 应用的包名要与商店后台一致；本项目历史上出现过 `com.example.*` 被拒，改包名要独立评估。

## 隐私政策（诗词闯关记单应用页，历史版本）

- 根地址：https://shine-zyy.github.io/publicPrivacy/
- 政策页：https://shine-zyy.github.io/publicPrivacy/privacy-policy.html

（该链接已用在诗词闯关记的上架资料里，**不要改动这个文件名或内容口径**；
新应用请用上面的统一页。）

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


---

## 识字闯关记（com.peanut.literacy）

汉字启蒙应用（开发者：张艳艳）的公开政策页：

- 隐私政策：https://shine-zyy.github.io/publicPrivacy/privacy-policy-literacy.html
- 儿童隐私政策：https://shine-zyy.github.io/publicPrivacy/child-privacy-literacy.html

（命名沿用 moodmonster 惯例，带 `-literacy` 后缀；两页互为相对链接。）

---

## 看谁反应快（com.runfast.app）

反应力与观察力小游戏（开发者：张艳艳）。**用的是统一政策页 + 参数**，
不再单独出 HTML：

- 隐私政策：https://shine-zyy.github.io/publicPrivacy/shared/privacy-policy.html?app=%E7%9C%8B%E8%B0%81%E5%8F%8D%E5%BA%94%E5%BF%AB&pkg=com.runfast.app
- 儿童隐私政策：https://shine-zyy.github.io/publicPrivacy/shared/child-privacy.html?app=%E7%9C%8B%E8%B0%81%E5%8F%8D%E5%BA%94%E5%BF%AB&pkg=com.runfast.app
- 儿童隐私协议：https://shine-zyy.github.io/publicPrivacy/shared/child-agreement.html?app=%E7%9C%8B%E8%B0%81%E5%8F%8D%E5%BA%94%E5%BF%AB&pkg=com.runfast.app

口径核对（该应用实测）：`module.json5` 的 `requestPermissions` 为空数组，
即**未申请任何权限、技术上无法联网**，与统一页里写的完全一致；
应用内删除路径为「设置 → 隐私与帮助 → 清除成绩记录」，也已写进统一页。
