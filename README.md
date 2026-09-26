# publicPrivacy

各应用的公开隐私政策页，通过 GitHub Pages 对外访问。

> **页面公开信息口径**：所有在线 HTML **只展示「应用名称」与「联系邮箱」**，
> 不展示包名、开发者姓名 / 主体名称与联系电话。新增或修改页面时请保持这一口径。

## 目录结构

```
index.html                        政策索引（列出所有页面）
shared/                           ★ 统一政策页：同一开发者系列应用共用
  privacy-policy.html             隐私政策
  child-privacy.html              儿童隐私政策
  child-agreement.html            儿童隐私协议（保留，但已不再从上面两页互相链接）
privacy-policy.html               诗词闯关记（历史单应用页，保持可用）
child-privacy.html
privacy-policy-literacy.html      识字闯关记
child-privacy-literacy.html
privacy-policy-moodmonster.html   心情小怪兽
child-privacy-moodmonster.html
```

## ★ 统一政策页（新应用优先用这套）

一份 HTML 同时服务多款应用：**应用名从 URL 参数读**，页面自己填进去。
所以同类应用只需复用同一组链接、换参数，不用再写一份政策。

三个固定链接（把 `APP` 换成应用名并做 URL 编码）：

```
https://shine-zyy.github.io/publicPrivacy/shared/privacy-policy.html?app=APP
https://shine-zyy.github.io/publicPrivacy/shared/child-privacy.html?app=APP
```

> ⚠️ 2026-09-26：应用内**只挂上面两条**。`child-agreement.html`（《儿童隐私协议》）文件保留，
> 但已从「隐私政策」第六节和「儿童隐私政策」页尾拿掉了互相链接 ——
> **「儿童隐私协议」与「儿童隐私政策」其实是同一个东西**，两处并列会让用户以为要看两份、
> 也会让审核认为文档之间存在口径冲突。

例（反应速答）：

```
https://shine-zyy.github.io/publicPrivacy/shared/privacy-policy.html?app=%E5%8F%8D%E5%BA%94%E9%80%9F%E7%AD%94
```

ArkTS 里生成（`AppCopy.ets` 已经这么写）：

```ts
const URL_BASE = 'https://shine-zyy.github.io/publicPrivacy/shared';
const URL_QUERY = `?app=${encodeURIComponent(APP_NAME)}`;
```

**注意**：

- 参数一定要带上。漏了 `app` 页面就显示兜底的「本应用」，审核会认为
  「政策页里的应用名与商店资料不一致」而驳回。
- 参数只带应用名，别在链接里塞别的（包名、开发者、电话一律不进页面）。
- 统一页里写的是「不收集任何个人信息、未申请任何权限」这一套口径。
  **如果某个应用的实际行为不一样（申请了权限、有联网、有第三方 SDK），
  就不要用它**，必须为该应用单独出一份政策页（见下面「单应用页」）。

## 隐私政策（诗词闯关记单应用页，历史版本）

- 根地址：https://shine-zyy.github.io/publicPrivacy/
- 政策页：https://shine-zyy.github.io/publicPrivacy/privacy-policy.html

（该链接已用在诗词闯关记的上架资料里，**不要改动这个文件名或内容口径**；
新应用请用上面的统一页。）

## 发布前必改（1 处：应用名）

页面里只有两项可变信息：**应用名称**（走 URL 参数，见上一节）与**联系邮箱**（写死在页面里）。

新应用接入时通常**不用改 HTML**，只要链接带上 `?app=应用名`；
只有当联系邮箱变了，才需要全仓库替换：

```bash
cd publicPrivacy
grep -rl 'shine_zyy@126.com' . | xargs sed -i '' 's/shine_zyy@126.com/you@example.com/g'
```

⚠️ 不要在页面里补回包名 / 开发者姓名 / 联系电话——这套页面刻意只留应用名与邮箱。

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

## 识字闯关记

识字应用，**用自己的单应用页**（不是 shared 页），已发布项目的政策链接指向这两个地址，
**不要删除或改名**（2026-09-27 因误更名导致线上 404，已恢复）：

- 隐私政策：https://shine-zyy.github.io/publicPrivacy/privacy-policy-literacy.html
- 儿童隐私政策：https://shine-zyy.github.io/publicPrivacy/child-privacy-literacy.html

---

## 拼音闯关记

拼音启蒙应用。**2026-09-26 起改用统一政策页 + 参数**（应用内设置页已同步改指向）：

- 隐私政策：https://shine-zyy.github.io/publicPrivacy/shared/privacy-policy.html?app=%E6%8B%BC%E9%9F%B3%E9%97%AF%E5%85%B3%E8%AE%B0
- 儿童隐私政策：https://shine-zyy.github.io/publicPrivacy/shared/child-privacy.html?app=%E6%8B%BC%E9%9F%B3%E9%97%AF%E5%85%B3%E8%AE%B0

> ⚠️ 本应用**在 `publicPrivacy/` 下没有单应用页**（`privacy-policy-pinyin.html` /
> `child-privacy-pinyin.html` 不应存在）。曾误把识字闯关记的 `-literacy` 两个文件更名成
> `-pinyin`，导致识字闯关记的线上链接 404；**2026-09-27 已回滚**：`-literacy` 两件恢复原状，
> `-pinyin` 两件删除。拼音闯关记只用上面两条 shared 链接，AGC 后台填写的政策地址须同上。

---

## 反应速答

反应力与观察力小游戏。**用的是统一政策页 + 参数**，不再单独出 HTML：

- 隐私政策：https://shine-zyy.github.io/publicPrivacy/shared/privacy-policy.html?app=%E5%8F%8D%E5%BA%94%E9%80%9F%E7%AD%94
- 儿童隐私政策：https://shine-zyy.github.io/publicPrivacy/shared/child-privacy.html?app=%E5%8F%8D%E5%BA%94%E9%80%9F%E7%AD%94
- 儿童隐私协议：https://shine-zyy.github.io/publicPrivacy/shared/child-agreement.html?app=%E5%8F%8D%E5%BA%94%E9%80%9F%E7%AD%94

口径核对（该应用实测）：`module.json5` 的 `requestPermissions` 为空数组，
即**未申请任何权限、技术上无法联网**，与统一页里写的完全一致；
应用内删除路径为「设置 → 隐私与帮助 → 清除成绩记录」，也已写进统一页。
