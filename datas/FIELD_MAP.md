# steambk 数据字段对照表（简化 ↔ 原字段）

> 适用数据：本目录 `list.json`（列表）与 `details/<id>.json`（详情）。
> 2026-11-17 起字段名简化为超短缩写；本表为「简化字段 ↔ 原字段」的完整对应关系。
> **保留全称的字段**：`mydowns` 字段名本身；`dn`（下载列表）与 `mydowns` 条目内的 `pan` / `share_url` / `qrcode` / `pwd`。
> 完整 HTML 来源锚点与抓取细节见 skill 文档：`/Users/HeSirCode/my-skills/games-copy/references/fields.md`。

## 一、list.json（列表，每页 20 条）

| 简化字段 | 原字段 | 类型 | 说明 | 示例值 |
|---|---|---|---|---|
| `id` | id（不变） | int | 站点文章 ID，即详情页 URL 中的数字 | `26012` |
| `my` | my（不变） | int | 该游戏 `mydowns` 数量统计，**默认 0**，转存回填后为 1 | `0` / `1` |
| `u` | url | str | 详情页链接 | `https://www.steambk.com/26012.html` |
| `t` | title | str | 完整标题（含版本号与"下载"后缀） | `拳破高墙/拳打墙壁（Punch The Walls）v1.0.3 免安装中文版下载` |
| `sg` | sign | str | 状态标记：`更新` / `新游` | `新游` |
| `rt` | rating | str | 评价：好评如潮/特别好评/好评/多半好评/褒贬不一/差评/暂无 等 | `特别好评` |
| `sz` | size | str | 游戏体积 | `549MB` / `32.5GB` |
| `yr` | release_year | str | 发行年份 | `2026` |
| `cv` | cover | str | 真实封面图（列表页 `src` 是占位图，此值取自 `data-src`） | `https://...header.jpg` |
| `ct` | category | str | 主分类（PC游戏区固定"PC游戏"） | `PC游戏` |
| `sub` | sub_category | str | 子分类 | `动作冒险` |
| `ssub` | sub_category_slug | str | 子分类拼音缩写；**可能缺失**（仅备用解析分支产出） | `dzmx` |
| `ex` | excerpt | str | 简介摘要（截断以 `...` 结尾） | `游戏介绍 这是一款...` |
| `tm` | time_text | str | 相对发布时间（站点原文） | `25分钟前` |
| `vw` | views | str | 浏览量（`k`=千、`w`=万） | `10.48w` |

## 二、details/<id>.json（详情，每游戏一个文件）

### 2.1 基础信息

| 简化字段 | 原字段 | 类型 | 说明 | 示例值 |
|---|---|---|---|---|
| `id` | id（不变） | int | 站点文章 ID | `26012` |
| `u` | url | str | 详情页地址 | `https://www.steambk.com/26012.html` |
| `t` | title | str | 完整标题 | 同列表 `t` |
| `sg` | sign | str | 状态标记：`更新` / `新游` | `更新` |
| `tm` | time_text | str | 相对发布时间 | `12小时前` |
| `vw` | views | str | 浏览量。**详情页站点无此元素，通常缺失**（列表页恒有） | `18.49w`（一般不出现） |
| `ct` | category | str | 主分类 | `PC游戏` |
| `sub` | sub_category | str | 子分类 | `动作冒险` |
| `tags` | tags（不变） | str[] | 标签列表 | `["剧情","奇幻","开放世界"]` |
| `sz` | game_size | str | 游戏体积 | `32.5GB` |
| `rt` | rating | str | 评价 | `暂无` |
| `ver` | version | str | 版本号（可能为 Build 号） | `v2.1.0` / `Build.26531378` |
| `rdate` | release_date | str | 发行日期（站点原文格式） | `2026 年 11 月 5 日` |
| `cv` | cover | str | 正文首图（与封面相同或标题横图，可能为空） | `https://...` |

### 2.2 正文区块（按 `<h2>` 切分转纯文本）

| 简化字段 | 原字段 | 类型 | 对应区块 | 说明 |
|---|---|---|---|---|
| `in` | intro | str | 游戏介绍 | 核心字段，基本必有 |
| `pt` | porting_notes | str | 移植版说明 | 仅移植类游戏有，否则空串 |
| `un` | update_notes | str | 更新内容 | 持续更新的游戏才有 |
| `vf` | version_info | str | 版本信息 | 一行式版本描述，基本必有 |
| `dl` | dlc_list | str[] | 包含DLC | 无 DLC 时空数组 |
| `bl` | buy_link | str | 正版购买 | Steam/官网跳转链接 |
| `sr` | system_requirements | dict | 系统需求 | **部分游戏整块缺失 → `{}`**；子键为中文（操作系统/处理器/内存/显卡/DirectX 版本/网络/存储空间等）+ `note`，勿假定齐全 |
| `sc` | screenshots | str[] | 游戏截图 | 截图 URL 列表，一般 5~90 张 |

### 2.3 视频

| 简化字段 | 原字段 | 类型 | 说明 |
|---|---|---|---|
| `vd` | videos | obj[] | 视频列表；条目 `{u, type, t}`（见下） |
| `vt` | video_titles | str[] | 视频分节标题（全部节名，任意分页可见） |

`vd` 条目对照：`url` → **`u`**、`title` → **`t`**；`type` 不变，三种取值：`hls`（自托管 m3u8）/ `bilibili_iframe`（B站嵌入）/ `mp4`（站内直链）。空数组 = 无预告视频，属正常。

### 2.4 下载盒子 `db`（原 download_box）

| 简化字段 | 原字段 | 类型 | 说明 | 示例值 |
|---|---|---|---|---|
| `db.ds[]` | download_box.types[] | obj[] | 网盘组条目数组 | 见下 |
| `db.ds[].pan` | pan_type | str | 网盘组名 | `移动云盘` / `其它网盘` |
| `db.ds[].pr` | price | str | 价格 | `5` / `免费` |
| `db.ds[].pr_u` | price_unit | str | 价格单位 | `推广币` / 空 |
| `db.ds[].idx` | download_index | int | erphpdown 分组号（1=付费组，2=免费组） | `2` |
| `db.note` | — | str | 下载区提示文本（截 500 字符） | `此资源购买后3天内可下载。...` |

### 2.5 下载列表 `dn`（原 downloads；仅 `--with-downloads` 或 `download` 命令产出）

条目内键名**保留全称**，仅 `group_index` 简化：

| 字段 | 原字段 | 类型 | 说明 | 示例值 |
|---|---|---|---|---|
| `pan` | pan（不变） | str | 网盘名 | `夸克` / `迅雷` / `百度` |
| `share_url` | share_url（不变） | str | 真实网盘分享链接（夸克/百度来自二维码 data 参数解码；迅雷两级 302 跟随） | `https://pan.quark.cn/s/10dc0c69e395` |
| `qrcode` | qrcode（不变） | str | 站点二维码图片地址（可能缺失） | `https://api.qrserver.com/...` |
| `pwd` | pwd（不变） | str | 提取码（夸克一般无） | `2p2y` |
| `gi` | group_index | int | 所属下载分组（同 `db.ds[].idx`） | `2` |

### 2.6 `mydowns`（用户自填预留字段，转存分享流水线回填）

初始为空数组；字段名与条目内 `pan`/`share_url`/`qrcode` **保留全称**（用户指定格式），新增 `click` 点击计数：

```json
{
  "pan": "夸克",
  "share_url": "https://pan.quark.cn/s/9b8a3c2661df",
  "qrcode": "https://api.qrserver.com/v1/create-qr-code/?size=140x140&data=https%3A%2F%2Fpan.quark.cn%2Fs%2F9b8a3c2661df",
  "click": 0
}
```

- `click`：该分享链接的点击/使用次数计数，**初始 0**，由下游前端/接口维护累加。
- 回填命令：`python3 /Users/HeSirCode/my-skills/games-copy/scripts/steambk_scraper.py mydown --id <id> --url "<自己的夸克分享链接>"`（自动同步列表 `my`；防呆拒绝站点原链接；同链接去重）。

### 2.7 `dn` 历史下载链接（内嵌在 dn 数组中，本地标记，无站点原字段）

**非站点数据标记**。dn 数组按新旧排序：**当前链接组在最前（最新），其后为历史条目，越近替换的越靠前**。重抓相同游戏（按 id 判断）时，若当前链接组与文件中已有组不同，被替换的旧当前组逐条附加 `ver`（替换前版本号）与 `tm`（当时页面时间）标记，插入到新当前组之后。

```json
"dn": [
  { "pan": "夸克", "share_url": "https://pan.quark.cn/s/当前链接", "gi": 2 },
  { "pan": "夸克", "share_url": "https://pan.quark.cn/s/旧链接", "gi": 2, "ver": "v1.0.2", "tm": "3天前" }
]
```

- **判别方式：条目含 `ver` 键即历史链接**；历史条目与当前条目唯一区别是多 `ver`/`tm` 两个键，其余结构同 2.5。
- 生成条件：仅当旧文件已有当前链接组（无 `ver` 键的条目），且其链接组指纹（按 `pan|share_url` 排序）与新抓结果不同时转换；**无变化、首次抓取、旧文件无 dn、新抓组无有效链接（全 error）、或本次未抓下载时 dn 原样保留**。
- 多次更新后 dn 会累积多组历史，越靠后越旧；前端渲染下载列表时应只展示**不含 `ver` 键**的当前组，历史组可折叠为「历史版本资源」。
- 何时查看：游戏更新后站点失效旧链接，需要找历史版本资源时按历史条目 `ver` 定位。

## 三、程序化迁移映射（JSON，可直接在前端消费）

```json
{
  "list": {
    "url": "u", "title": "t", "sign": "sg", "rating": "rt",
    "size": "sz", "release_year": "yr", "cover": "cv",
    "category": "ct", "sub_category": "sub", "sub_category_slug": "ssub",
    "excerpt": "ex", "time_text": "tm", "views": "vw"
  },
  "detail": {
    "url": "u", "title": "t", "sign": "sg", "time_text": "tm", "views": "vw",
    "category": "ct", "sub_category": "sub",
    "game_size": "sz", "rating": "rt", "version": "ver", "release_date": "rdate",
    "cover": "cv", "intro": "in", "porting_notes": "pt", "update_notes": "un",
    "version_info": "vf", "dlc_list": "dl", "buy_link": "bl",
    "system_requirements": "sr", "screenshots": "sc",
    "videos": "vd", "video_titles": "vt", "download_box": "db", "downloads": "dn"
  },
  "video_entry": { "url": "u", "title": "t" },
  "download_box_entry": { "pan_type": "pan", "price": "pr", "price_unit": "pr_u", "download_index": "idx" },
  "downloads_entry": { "group_index": "gi" },
  "mydowns_entry": { "click(新增)": "click", "pan/share_url/qrcode": "保留全称" }
}
```

不变字段：`id`、`my`、`tags`、`type`（vd 条目）、`dn` 条目内 `pan`/`share_url`/`pwd`/`qrcode`、`mydowns` 条目内 `pan`/`share_url`/`qrcode`。**dn 数组内嵌历史下载链接**（条目附加 `ver`/`tm` 键即为历史，见 2.7），无独立字段。

## 四、异常字段

| 字段 | 出现条件 | 说明 |
|---|---|---|
| `error` | 该条抓取失败 | 条目仅含 `id`/`u`/`error`，可按 id 重跑 |
| `dn[].error` | 中转页无可用下载（需登录/已失效） | 如 `{"pan":"index=1","error":"中转页无可用下载(可能需登录/已失效)"}` |
| `share_url: null` | 迅雷时效签名过期 | 重跑 `download` 命令重新获取 |
