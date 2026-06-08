# WeChat Pay 加盟店パートナー研修 / 服务商培训 H5

面向日本服务商一线员工的微信支付物料铺设培训小测试。日中双语、纯静态单文件、可托管于 GitHub Pages。

## 文件

- `index.html` — 一切都在这里：登录 + 14 题 + 结果页 + 错题回顾。无外部依赖，可离线打开。
  - 前 4 题：真实驳回截图（Base64 内嵌，含 yongwen 水印，仅内部使用）
  - 后 10 题：SVG 示意图 + 文字规则题
- `bad-cases-template.xlsx` — Bad Cases 收集模板（README / Master / Dashboard / _Lookup 4 个 sheet）
- `cases/` — 4 张真实图源文件（已 .gitignore，不上 GitHub）
- `build_badcases_template.py` — 生成 Excel 模板的脚本，要改字段/类型可以重跑

## ⚠️ 安全注意

当前 index.html 内嵌了 4 张真实驳回截图（Base64），水印 `yongwen` 是腾讯内部审核系统名。

- ✅ 适用：内部团队评审、上级演示、自己本地测试
- ❌ 不适用：直接推 GitHub Public、发给海外服务商

**对外发布时**，先在 index.html 中：
1. 找到 `const REJ_IMG = {` 整段，删掉
2. 找到 `id: 101` ~ `id: 104` 4 道题，删掉
3. 此时 H5 自动回退到 10 题 SVG 示意图版本，可安全发布

## 账号清单（13 家服务商）

| 账号 | 密码 | 服务商 |
|---|---|---|
| `lian` | `wp-lian-2026` | Lian |
| `spring` | `wp-spring-2026` | Spring |
| `wonder` | `wp-wonder-2026` | Wonder |
| `jr-east` | `wp-jreast-2026` | JR East / JR东 |
| `weegent` | `wp-weegent-2026` | Weegent |
| `koun` | `wp-koun-2026` | 行云 / Koun |
| `widein` | `wp-widein-2026` | Widein |
| `infinite` | `wp-infinite-2026` | Infinite |
| `retty` | `wp-retty-2026` | Retty |
| `office-solution` | `wp-officesolution-2026` | Office Solution |
| `playon` | `wp-playon-2026` | Playon |
| `tierra` | `wp-tierra-2026` | Tierra |
| `imprex` | `wp-imprex-2026` | Imprex |

> 同一服务商共用一个账号，不同员工各自登录后填自己的名字。

## 部署到 GitHub Pages（15 分钟）

1. 注册或使用对外账号（建议新建 `wp-japan-training`）
2. 新建 Public 仓库，比如 `material-placement`
3. 将 `index.html` 拖到仓库根目录
4. 仓库 Settings → Pages → Source 选 `main` 分支 `/(root)`，保存
5. 等 1 分钟，访问 `https://<账号>.github.io/material-placement/`

发给服务商的话术（日文）：

```
お疲れ様です。
WeChat Pay 加盟店物料配置の研修サイトを作成しました。
所要時間：約 3 分

URL：https://xxx.github.io/material-placement/
アカウント：lian（御社用）
パスワード：wp-lian-2026

完了後、結果画面のスクリーンショットをこちらまで送ってください。
Safari または Chrome での閲覧を推奨します。
```

## 修改 / 维护

**改账号密码**：编辑 `index.html` 顶部 `const ACCOUNTS = {...}` 一段即可。

**改题目**：编辑 `const QUESTIONS = [...]` 数组。题目结构：

```js
{
  id: 11,
  type: 'choice',         // 'choice' 或 'image'
  stem_ja: '日本語の問題文',
  stem_cn: '中文题干',
  options: [
    { label_ja: '...', label_cn: '...' },  // 文字题
    // 或 { img: svgScene('ok'), label_ja: 'A', label_cn: '说明' }  // 图片题
  ],
  multi: false,           // 多选题设为 true
  answer: 0,              // 单选填索引；多选填数组 [0, 1]
  explain_ja: '解説（日本語）',
  explain_cn: '解析（中文）',
  why_ja: '...',          // 可选：错因详解
  why_cn: '...'
}
```

**替换占位 SVG 为真实门店照片**：

把 `svgScene('ok')` 这种调用替换为内嵌图片字符串：

```js
{ img: '<img src="data:image/jpeg;base64,/9j/4AAQ..." style="width:100%;height:100%;object-fit:cover">', label_ja: 'A' }
```

或者使用外部图片（GitHub 仓库下放一个 `images/` 目录）：

```js
{ img: '<img src="images/ok-shelf.jpg" style="width:100%;height:100%;object-fit:cover">', label_ja: 'A' }
```

## 不收数据怎么"看"完成情况

服务商答完结果页时，页面会提示**截图发回**。你收到截图后人工确认即可。

页面右上角始终显示「姓名 · 服务商」，截图自带署名。

## 安全说明

- 这是 Public 仓库，HTML 源码（含账号密码、题目答案）任何人可见，**不能放任何内部规则、激励金额、商户全名**
- 题目里的 A 类/B 类规则只点到"有上限"程度，具体数字以合同/政策为准
- 一年后建议把密码尾号改成 `2027`（编辑 `ACCOUNTS` 一处即可）
