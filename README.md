# 📜 plugin_jinriyunshi ｜NoneBot 今日运势插件

一个基于 NoneBot 2 的娱乐型插件，为用户提供每日一次的“今日人品/运势”签文。结合玄学风格的运势等级体系，支持自动缓存与每日清除机制，增强趣味性与可持续体验。

---

## ✨ 功能简介

- 生成用户每日运势（人品星级 + 运势签文 + 图像）
- 使用 Wallhaven API 自动爬取图像
- 图像扩充去重机制（`imagehash` + `md5`）
- 图池每日凌晨自动刷新，也支持管理员手动刷新
- 每人每天只能触发一次运势，避免刷屏
- 支持私聊命令触发图池扩充

---

## 🖼 使用示例

插件使用效果如下：

![示例1](demo/demo1.png)
![示例2](demo/demo2.png)


展示了用户发送 `.今日运势` 后收到的图文信息结构。

---

## 📥 指令说明

### 用户指令

| 指令 | 说明 |
|------|------|
| `.今日运势` 或 `.今日人品` | 随机生成当日运势等级（0~8），附加一段签文和随机图像，每日仅可触发一次 |

### 管理员指令（需在私聊中触发）

| 指令 | 说明 |
|------|------|
| `.扩充图池` | 手动执行图池刷新，触发 Wallhaven 抓图操作，异步执行不会阻塞主线程 |

---

## 🧠 插件原理说明

### 运势生成逻辑

- 70% 概率采用「四维组合」（财运/姻缘/事业/人品）进行评分，每项范围 0~2
- 否则完全随机生成等级后，回推合法组合
- 每个等级（0~8）有三段预设签文，随机抽取一段
- 星级以 `★` 和 `☆` 表示

### 图池图片逻辑

- 每天凌晨 3 点自动刷新图池
- 抓取来源：Wallhaven API
- 标签：genshin-impact, honkai-star-rail, 等二次元游戏
- 每个标签获取 5 张图片，使用 `imagehash` + `md5` 判重
- 成功下载的图片保存在 `cache/wallhaven_download/`

### 缓存机制

- 每日运势结果缓存至 `cache/daily_cache.json`
- 每个用户每日最多获取一次运势结果
- 每日自动清空旧缓存数据

### 图池去重机制

- 使用 `PIL` + `imagehash` 计算 aHash
- 使用 `hashlib` 生成图片字节的 md5 值
- 重复图片会被自动跳过，避免存入图池

---

## 🗂 项目结构

```bash
plugin_jinriyunshi/
├── __init__.py
├── cache/
│   ├── daily_cache.json           # 每日运势缓存
│   ├── wallhaven_download/       # 下载的图像池文件夹
│   └── .hash_cache.json          # 图像去重记录
├── demo/
│   ├── demo1.png                  # 使用示意图1
│   └── demo2.png                  # 使用示意图2
└── README.md                     # 本文件
```

---

## ⚙ 使用建议

- 请将管理员 QQ 填入 `ADMIN_QQ_LIST`
- 图像池为空时用户将收到提示，可提前补图或手动 `.扩充图池`
- 建议定期清理 `wallhaven_download` 中旧图片（可手动或自动化）

---

## 📌 免责声明

- 图像内容均来自公开图库 [wallhaven.cc](https://wallhaven.cc)，请遵守其使用协议
- 本插件生成运势内容仅为娱乐，请勿迷信，科学生活

---

## 📮 联系方式

如需反馈或建议，欢迎提 Issue 或 PR！
