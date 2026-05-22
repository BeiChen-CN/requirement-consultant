# HTML 可视化模板指南

本文件为 requirement-consultant skill 的 HTML 可视化提供参考模板。

## 1. 双端预览页面骨架

每个 HTML 文件应在同一页面中并排展示移动端（375px）和桌面端（1280px）效果：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<title>[方案名称]</title>
<style>
  body { font-family: -apple-system, "Microsoft YaHei", sans-serif; margin: 0; padding: 20px; background: #f5f5f5; }
  .preview-container { display: flex; gap: 20px; justify-content: center; flex-wrap: wrap; }
  .preview-box { background: white; border-radius: 8px; overflow: hidden; box-shadow: 0 2px 12px rgba(0,0,0,0.1); }
  .preview-label { text-align: center; padding: 8px; background: #333; color: white; font-size: 14px; }
  .mobile { width: 375px; }
  .desktop { width: 1280px; }
  .mobile iframe, .desktop iframe { border: none; width: 100%; height: 800px; }
</style>
</head>
<body>
<h2 style="text-align:center">[方案名称]</h2>
<div class="preview-container">
  <div class="preview-box">
    <div class="preview-label">移动端 375px</div>
    <div class="mobile"><iframe srcdoc='<div id="app"><!-- 移动端内容 --></div>'></iframe></div>
  </div>
  <div class="preview-box">
    <div class="preview-label">桌面端 1280px</div>
    <div class="desktop"><iframe srcdoc='<div id="app"><!-- 桌面端内容 --></div>'></iframe></div>
  </div>
</div>
</body>
</html>
```

## 2. 动态风格生成框架

风格不是固定模板，而是根据用户回答动态生成。以下是 AI 构建风格时使用的参数维度。

### 2.1 风格参数维度

每个方案由以下参数组合决定：

| 维度 | 低 | 中 | 高 |
|------|-----|-----|-----|
| **色彩饱和度** | 低饱和（灰调、莫兰迪色系） | 中饱和（柔和但有色彩感） | 高饱和（鲜明、对比强烈） |
| **信息密度** | 极简留白（大面积空白） | 适中（平衡内容与留白） | 信息丰富（紧凑排列） |
| **视觉层次** | 扁平（纯色、无阴影） | 微立体（轻阴影、浅渐变） | 强立体（重阴影、卡片层叠） |
| **排版节奏** | 宽松（大间距、大字号） | 标准 | 紧凑（小间距、字号适中） |
| **圆角大小** | 小圆角/直角（0-4px） | 中圆角（8-12px） | 大圆角（16-24px） |
| **主色倾向** | 冷色（蓝/紫/灰） | 中性（黑白/深蓝） | 暖色（红/橙/黄/绿） |

### 2.2 从用户回答推导参数

| 用户表达 | 推导 |
|----------|------|
| "像苹果官网" | 低饱和 + 极简留白 + 扁平 + 宽松 + 小圆角 + 冷色 |
| "像小红书" | 高饱和 + 适中密度 + 微立体 + 标准排版 + 大圆角 + 暖色 |
| "像银行 APP" | 低饱和 + 信息丰富 + 微立体 + 紧凑 + 小圆角 + 冷色 |
| "温暖亲切" | 暖色倾向 + 大圆角 + 微立体 |
| "冷静专业" | 冷色倾向 + 小圆角 + 扁平或微立体 |
| "颜色丰富" | 高饱和 + 多辅色 |
| "素净" | 低饱和 + 少辅色 |
| 用户提到喜欢的 APP | AI 分析该 APP 的视觉特征，映射到参数 |

### 2.3 生成 3 套方案的方法

1. 根据用户回答确定一组**基准参数**
2. 在基准参数基础上，沿 2-3 个维度做差异化偏移，生成 3 套方案：
   - **方案 A：** 基准参数（最接近用户描述）
   - **方案 B：** 在色彩饱和度或信息密度上偏移一个档位
   - **方案 C：** 在视觉层次或排版节奏上偏移一个档位
3. 确保 3 套方案之间有明显差异，但都在用户偏好的大方向内

### 2.4 配色方案生成

确定风格方向后，配色从主色自动派生：

```
主色 → 选择（基于用户偏好推导的色系倾向）
辅色 → 主色的互补色或同类色（自动计算）
背景 → 白色 / 浅灰 / 暖白（根据主色冷暖调整）
文字 → 深灰/黑色（确保与背景对比度 ≥ 4.5:1）
强调色 → 主色的高饱和变体（用于按钮、链接）
```

## 3. 常用组件模板

### 导航栏
```html
<nav style="display:flex; align-items:center; justify-content:space-between; padding:12px 24px; background:[主色]; color:white;">
  <div style="font-size:20px; font-weight:bold;">[品牌名]</div>
  <div style="display:flex; gap:20px;">
    <a href="#" style="color:white; text-decoration:none;">首页</a>
    <a href="#" style="color:white; text-decoration:none;">功能</a>
    <a href="#" style="color:white; text-decoration:none;">关于</a>
  </div>
</nav>
```

### 卡片
```html
<div style="background:white; border-radius:[圆角]; padding:20px; box-shadow:0 2px 8px rgba(0,0,0,0.08); margin:12px;">
  <h3 style="margin:0 0 8px; color:[文字色];">[标题]</h3>
  <p style="color:#666; margin:0;">[描述]</p>
</div>
```

### 按钮
```html
<button style="background:[主色]; color:white; border:none; padding:12px 24px; border-radius:[圆角]; font-size:16px; cursor:pointer;">[按钮文字]</button>
```

### 输入框
```html
<input type="text" placeholder="[提示文字]" style="width:100%; padding:12px; border:1px solid #ddd; border-radius:[圆角]; font-size:16px; box-sizing:border-box;">
```

## 4. 使用要点

- 所有样式内联，不使用外部资源
- 占位文本用项目相关内容，不用 lorem ipsum
- 图片用纯色块 + 文字描述代替
- 颜色对比度符合可读性标准（WCAG AA）
- 移动端注意触控区域不小于 44x44px
