# 鸿蒙应用墙页面设计文档

**日期**：2026-04-10  
**仓库**：app-harmony-wall  
**托管方式**：GitHub Pages（静态，无服务端）

---

## 背景与目标

在各鸿蒙 APP 内添加"更多应用"入口，点击后由系统浏览器打开本页面，实现 APP 矩阵内的交叉曝光。

**约束**：
- APP 本身不发网络请求，仅将固定 URL 扔给系统浏览器，不需要 INTERNET 权限
- 页面由开发者自己维护，新增或下架 APP 只需更新本页面，各 APP 无需发版
- 纯中文界面

---

## 文件结构

```
app-harmony-wall/
├── index.html          # 页面模板，JS 读取 apps.json 动态渲染卡片
├── apps.json           # APP 数据，新增/下架唯一需要修改的文件
└── icons/
    ├── xxsecret.png    # 来源：XXSecret/AppScope/resources/base/media/foreground.png
    ├── eatwhat.png     # 来源：hm-eatwhat/AppScope/resources/base/media/foreground.png
    ├── sandbank.png    # 来源：app-002-SandBank/sandbank-harmony/AppScope/.../foreground.png
    ├── breathing.png   # 来源：app-003-breathing/breathing-Harmony/AppScope/.../background.png
    │                   #       （foreground.png 内容几乎透明，改用 background.png）
    ├── pixelgo.png     # 来源：app-008-PixelGo/pixelGo-harmony/AppScope/.../foreground.png
    └── subkiller.png   # 来源：app-009-SubKiller/subkiller-harmony/AppScope/.../foreground.png
```

---

## apps.json 数据格式

```json
[
  {
    "name": "密语匣子",
    "bundleName": "com.xun.xxsecret",
    "description": "离线加密工具，把文字变成只有你知道的神秘密语",
    "tags": ["工具", "隐私"],
    "icon": "icons/xxsecret.png"
  },
  {
    "name": "决定今天吃什么",
    "bundleName": "com.xun.eatwhat2",
    "description": "随机、转盘、多人投票，终结选餐纠结",
    "tags": ["生活", "决策"],
    "icon": "icons/eatwhat.png"
  },
  {
    "name": "聚沙攒钱",
    "bundleName": "com.xun.sandbank",
    "description": "把愿望拆成小计划，本地记录存钱进度与节奏",
    "tags": ["理财", "习惯"],
    "icon": "icons/sandbank.png"
  },
  {
    "name": "呼吸视界",
    "bundleName": "com.xun.breathing",
    "description": "多种训练模式搭配沉浸视觉，随时放松减压",
    "tags": ["健康", "放松"],
    "icon": "icons/breathing.png"
  },
  {
    "name": "像素征途",
    "bundleName": "com.xun.pixelgo",
    "description": "以真实定位驱动，把日常出行沉淀为可视化城市记录",
    "tags": ["探索", "记录"],
    "icon": "icons/pixelgo.png"
  },
  {
    "name": "订阅斩",
    "bundleName": "com.xun.subkiller",
    "description": "集中管理订阅与周期付费，清晰追踪续费时间与费用",
    "tags": ["工具", "理财"],
    "icon": "icons/subkiller.png"
  }
]
```

AppGallery 链接由 JS 拼接：
```
https://appgallery.huawei.com/app/detail?id={bundleName}
```

---

## 页面设计

### 整体布局

- 页面背景：浅灰 `#f5f5f5`
- 最大宽度：480px，水平居中
- 顶部页眉：APP 名"我的鸿蒙应用"，无副标题
- 卡片列表：单列，垂直排列

### 卡片结构（每个 APP 一张）

```
┌─────────────────────────────────────┐
│  [图标 72px]  APP 名（粗体）        │
│               [标签1] [标签2]       │
│               一句话简介            │
│                      [在应用市场查看]│
└─────────────────────────────────────┘
```

- 图标：72×72px，圆角 16px，左对齐
- APP 名：`font-size: 17px`，`font-weight: 600`
- 标签：小胶囊，`font-size: 12px`，浅灰背景 `#ebebeb`，深灰文字
- 简介：`font-size: 14px`，次级灰色文字
- 按钮：`在应用市场查看`，卡片底部右对齐，深色填充（`#1a1a1a`），白色文字，点击跳转 AppGallery
- 卡片：白色背景，`border-radius: 16px`，轻阴影

### 响应式

- 屏幕宽度 < 600px：全宽，左右 padding 16px（手机端）
- 屏幕宽度 ≥ 600px：内容居中，最大宽度 480px（桌面查看也不难看）

---

## 新增 / 下架 APP 流程

**新增**：
1. 从新 APP 的鸿蒙工程复制图标文件到 `icons/`
2. 在 `apps.json` 末尾追加一条记录
3. 推送到 GitHub，页面自动更新

**下架**：
1. 从 `apps.json` 删除对应条目（图标文件可保留）
2. 推送即生效

---

## 当前上架 APP 一览（6 款）

| 显示名 | 包名 | 图标来源 |
|--------|------|----------|
| 密语匣子 | com.xun.xxsecret | foreground.png |
| 决定今天吃什么 | com.xun.eatwhat2 | foreground.png |
| 聚沙攒钱 | com.xun.sandbank | foreground.png |
| 呼吸视界 | com.xun.breathing | background.png（foreground 透明） |
| 像素征途 | com.xun.pixelgo | foreground.png |
| 订阅斩 | com.xun.subkiller | foreground.png |
