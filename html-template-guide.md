# HTML 可视化模板指南

本文件为 requirement-consultant skill 的 HTML 可视化提供参考模板和规范。

## 1. 三端预览页面骨架

每个 HTML 文件应在同一页面中并排展示移动端（375px）、平板端（768px）和桌面端（1280px）效果：

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
  .tablet { width: 768px; }
  .desktop { width: 1280px; }
  .mobile iframe, .tablet iframe, .desktop iframe { border: none; width: 100%; height: 800px; }
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
    <div class="preview-label">平板端 768px</div>
    <div class="tablet"><iframe srcdoc='<div id="app"><!-- 平板端内容 --></div>'></iframe></div>
  </div>
  <div class="preview-box">
    <div class="preview-label">桌面端 1280px</div>
    <div class="desktop"><iframe srcdoc='<div id="app"><!-- 桌面端内容 --></div>'></iframe></div>
  </div>
</div>
</body>
</html>
```

## 2. 设计规范

### 2.1 字号层级

| 层级 | 桌面端（≥1024px） | 平板端（768-1023px） | 移动端（≤767px） | 用途 |
|------|-------------------|---------------------|-----------------|------|
| H1 | 32-36px | 28-32px | 24-28px | 页面主标题 |
| H2 | 24-28px | 22-26px | 20-24px | 区块标题 |
| H3 | 18-20px | 18px | 16-18px | 卡片标题 |
| Body | 16px | 16px | 14-16px | 正文 |
| Small | 14px | 14px | 12-14px | 辅助文字 |
| Caption | 12px | 12px | 11-12px | 标签、时间戳 |

行高建议：正文 1.5-1.6，标题 1.2-1.3。

### 2.2 触控区域

**所有可点击元素在移动端不小于 44×44px。** 这是硬性规范，不是建议。

实现方式：
- 按钮：直接设置 `min-height: 44px; min-width: 44px;`
- 文字链接：用 `padding` 扩大点击区域，而非只靠文字大小
- 图标按钮：图标本身可以小，但外层容器必须 ≥ 44×44px
- 列表项：每行高度 ≥ 48px，确保手指不误触

```html
<!-- 错误：点击区域太小 -->
<a href="#" style="font-size:14px;">查看更多</a>

<!-- 正确：扩大点击区域 -->
<a href="#" style="display:inline-block; padding:12px 16px; font-size:14px; min-height:44px; line-height:20px;">查看更多</a>
```

### 2.3 响应式断点

```
移动端：  ≤ 767px   （单列布局，底部导航，全屏卡片）
平板端：  768-1023px （两列布局，可折叠侧边栏）
桌面端：  ≥ 1024px  （多列布局，固定侧边栏，宽幅内容区）
```

### 2.4 配色方案生成

确定风格方向后，配色从主色自动派生：

```
主色 → 选择（基于用户偏好推导的色系倾向）
辅色 → 主色的互补色或同类色（自动计算）
背景 → 白色 / 浅灰 / 暖白（根据主色冷暖调整）
文字 → 深灰/黑色（确保与背景对比度 ≥ 4.5:1）
强调色 → 主色的高饱和变体（用于按钮、链接）
```

### 2.5 深色模式配色（可选）

当项目适合深色模式时（音乐类、工具类、游戏类），额外提供深色方案：

```
背景 → #1a1a2e / #16213e / #0f0f0f（深蓝/深灰/纯黑）
卡片 → #2a2a3e / #1e293b（比背景稍亮）
文字 → #e0e0e0 / #f0f0f0（浅灰/近白）
主色 → 保持不变或提高亮度 10-15%
强调色 → 保持不变
边框 → #3a3a4e（低对比度分隔线）
```

## 3. 动态风格生成框架

风格不是固定模板，而是根据用户回答动态生成。以下是 AI 构建风格时使用的参数维度。

### 3.1 风格参数维度

每个方案由以下参数组合决定：

| 维度 | 低 | 中 | 高 |
|------|-----|-----|-----|
| **色彩饱和度** | 低饱和（灰调、莫兰迪色系） | 中饱和（柔和但有色彩感） | 高饱和（鲜明、对比强烈） |
| **信息密度** | 极简留白（大面积空白） | 适中（平衡内容与留白） | 信息丰富（紧凑排列） |
| **视觉层次** | 扁平（纯色、无阴影） | 微立体（轻阴影、浅渐变） | 强立体（重阴影、卡片层叠） |
| **排版节奏** | 宽松（大间距、大字号） | 标准 | 紧凑（小间距、字号适中） |
| **圆角大小** | 小圆角/直角（0-4px） | 中圆角（8-12px） | 大圆角（16-24px） |
| **主色倾向** | 冷色（蓝/紫/灰） | 中性（黑白/深蓝） | 暖色（红/橙/黄/绿） |

### 3.2 从用户回答推导参数

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

### 3.3 生成 3 套方案的方法

1. 根据用户回答确定一组**基准参数**
2. 在基准参数基础上，沿 2-3 个维度做差异化偏移，生成 3 套方案：
   - **方案 A：** 基准参数（最接近用户描述）
   - **方案 B：** 在色彩饱和度或信息密度上偏移一个档位
   - **方案 C：** 在视觉层次或排版节奏上偏移一个档位
3. 确保 3 套方案之间有明显差异，但都在用户偏好的大方向内

## 4. 组件模板库

所有样式内联，不使用外部资源。占位文本用项目相关内容，不用 lorem ipsum。图片用纯色块 + 文字描述代替。颜色对比度符合可读性标准（WCAG AA）。

### 导航栏

```html
<!-- 桌面端导航 -->
<nav style="display:flex; align-items:center; justify-content:space-between; padding:12px 24px; background:[主色]; color:white;">
  <div style="font-size:20px; font-weight:bold;">[品牌名]</div>
  <div style="display:flex; gap:20px;">
    <a href="#" style="color:white; text-decoration:none;">首页</a>
    <a href="#" style="color:white; text-decoration:none;">功能</a>
    <a href="#" style="color:white; text-decoration:none;">关于</a>
  </div>
</nav>

<!-- 移动端导航（汉堡菜单） -->
<nav style="display:flex; align-items:center; justify-content:space-between; padding:12px 16px; background:[主色]; color:white;">
  <div style="font-size:18px; font-weight:bold;">[品牌名]</div>
  <div style="font-size:24px; cursor:pointer; padding:8px;">☰</div>
</nav>
```

### 底部导航栏（移动端）

```html
<nav style="position:fixed; bottom:0; left:0; right:0; display:flex; justify-content:space-around; background:white; border-top:1px solid #eee; padding:8px 0; z-index:100;">
  <a href="#" style="display:flex; flex-direction:column; align-items:center; text-decoration:none; color:[主色]; min-width:44px; min-height:44px; justify-content:center;">
    <span style="font-size:20px;">🏠</span>
    <span style="font-size:11px; margin-top:2px;">首页</span>
  </a>
  <a href="#" style="display:flex; flex-direction:column; align-items:center; text-decoration:none; color:#999; min-width:44px; min-height:44px; justify-content:center;">
    <span style="font-size:20px;">🔍</span>
    <span style="font-size:11px; margin-top:2px;">搜索</span>
  </a>
  <a href="#" style="display:flex; flex-direction:column; align-items:center; text-decoration:none; color:#999; min-width:44px; min-height:44px; justify-content:center;">
    <span style="font-size:20px;">👤</span>
    <span style="font-size:11px; margin-top:2px;">我的</span>
  </a>
</nav>
```

### 侧边栏（桌面端）

```html
<aside style="width:240px; min-height:100vh; background:#f8f9fa; border-right:1px solid #eee; padding:20px 0; position:fixed; left:0; top:0;">
  <div style="padding:16px 20px; font-size:18px; font-weight:bold; color:[主色];">[品牌名]</div>
  <a href="#" style="display:block; padding:12px 20px; color:[主色]; background:rgba([主色RGB],0.1); text-decoration:none; border-right:3px solid [主色];">首页</a>
  <a href="#" style="display:block; padding:12px 20px; color:#666; text-decoration:none;">功能</a>
  <a href="#" style="display:block; padding:12px 20px; color:#666; text-decoration:none;">设置</a>
  <a href="#" style="display:block; padding:12px 20px; color:#666; text-decoration:none;">帮助</a>
</aside>
<!-- 主内容区需要 margin-left:240px -->
```

### 卡片

```html
<!-- 基础卡片 -->
<div style="background:white; border-radius:[圆角]; padding:20px; box-shadow:0 2px 8px rgba(0,0,0,0.08); margin:12px;">
  <h3 style="margin:0 0 8px; color:[文字色];">[标题]</h3>
  <p style="color:#666; margin:0;">[描述]</p>
</div>

<!-- 带图片的卡片 -->
<div style="background:white; border-radius:[圆角]; overflow:hidden; box-shadow:0 2px 8px rgba(0,0,0,0.08); margin:12px;">
  <div style="height:160px; background:[辅色]; display:flex; align-items:center; justify-content:center; color:white; font-size:14px;">[图片占位]</div>
  <div style="padding:16px;">
    <h3 style="margin:0 0 8px; color:[文字色];">[标题]</h3>
    <p style="color:#666; margin:0; font-size:14px;">[描述]</p>
  </div>
</div>

<!-- 可点击卡片（增加悬停提示） -->
<div style="background:white; border-radius:[圆角]; padding:20px; box-shadow:0 2px 8px rgba(0,0,0,0.08); margin:12px; cursor:pointer; transition:box-shadow 0.2s;">
  <h3 style="margin:0 0 8px; color:[文字色];">[标题]</h3>
  <p style="color:#666; margin:0;">[描述]</p>
  <span style="color:[主色]; font-size:14px; margin-top:12px; display:inline-block;">查看详情 →</span>
</div>
```

### 按钮

```html
<!-- 主按钮 -->
<button style="background:[主色]; color:white; border:none; padding:12px 24px; border-radius:[圆角]; font-size:16px; cursor:pointer; min-height:44px;">[按钮文字]</button>

<!-- 次按钮 -->
<button style="background:white; color:[主色]; border:2px solid [主色]; padding:12px 24px; border-radius:[圆角]; font-size:16px; cursor:pointer; min-height:44px;">[按钮文字]</button>

<!-- 文字按钮 -->
<button style="background:transparent; color:[主色]; border:none; padding:12px 16px; font-size:16px; cursor:pointer; min-height:44px; text-decoration:underline;">[按钮文字]</button>

<!-- 危险按钮 -->
<button style="background:#e74c3c; color:white; border:none; padding:12px 24px; border-radius:[圆角]; font-size:16px; cursor:pointer; min-height:44px;">[删除操作]</button>

<!-- 禁用按钮 -->
<button style="background:#ccc; color:#999; border:none; padding:12px 24px; border-radius:[圆角]; font-size:16px; cursor:not-allowed; min-height:44px;">[不可点击]</button>

<!-- 加载按钮 -->
<button style="background:[主色]; color:white; border:none; padding:12px 24px; border-radius:[圆角]; font-size:16px; cursor:wait; min-height:44px; opacity:0.7;">加载中...</button>
```

### 输入框

```html
<!-- 基础输入框 -->
<div style="margin-bottom:16px;">
  <label style="display:block; margin-bottom:4px; font-size:14px; color:#333;">[标签]</label>
  <input type="text" placeholder="[提示文字]" style="width:100%; padding:12px; border:1px solid #ddd; border-radius:[圆角]; font-size:16px; box-sizing:border-box; min-height:44px;">
</div>

<!-- 带错误提示的输入框 -->
<div style="margin-bottom:16px;">
  <label style="display:block; margin-bottom:4px; font-size:14px; color:#333;">[标签]</label>
  <input type="text" value="[错误内容]" style="width:100%; padding:12px; border:2px solid #e74c3c; border-radius:[圆角]; font-size:16px; box-sizing:border-box; min-height:44px;">
  <span style="font-size:12px; color:#e74c3c; margin-top:4px; display:block;">[错误提示文字]</span>
</div>

<!-- 文本域 -->
<div style="margin-bottom:16px;">
  <label style="display:block; margin-bottom:4px; font-size:14px; color:#333;">[标签]</label>
  <textarea placeholder="[提示文字]" rows="4" style="width:100%; padding:12px; border:1px solid #ddd; border-radius:[圆角]; font-size:16px; box-sizing:border-box; resize:vertical;"></textarea>
</div>
```

### 多字段表单

```html
<div style="max-width:480px; margin:0 auto; padding:24px; background:white; border-radius:[圆角]; box-shadow:0 2px 8px rgba(0,0,0,0.08);">
  <h2 style="margin:0 0 24px; color:[文字色];">[表单标题]</h2>

  <div style="margin-bottom:16px;">
    <label style="display:block; margin-bottom:4px; font-size:14px; color:#333;">姓名</label>
    <input type="text" placeholder="请输入姓名" style="width:100%; padding:12px; border:1px solid #ddd; border-radius:[圆角]; font-size:16px; box-sizing:border-box; min-height:44px;">
  </div>

  <div style="margin-bottom:16px;">
    <label style="display:block; margin-bottom:4px; font-size:14px; color:#333;">手机号</label>
    <input type="tel" placeholder="请输入手机号" style="width:100%; padding:12px; border:1px solid #ddd; border-radius:[圆角]; font-size:16px; box-sizing:border-box; min-height:44px;">
  </div>

  <div style="margin-bottom:16px;">
    <label style="display:block; margin-bottom:4px; font-size:14px; color:#333;">备注</label>
    <textarea placeholder="有什么想说的..." rows="3" style="width:100%; padding:12px; border:1px solid #ddd; border-radius:[圆角]; font-size:16px; box-sizing:border-box; resize:vertical;"></textarea>
  </div>

  <button style="width:100%; background:[主色]; color:white; border:none; padding:14px; border-radius:[圆角]; font-size:16px; cursor:pointer; min-height:44px;">提交</button>
</div>
```

### 表格 / 数据列表

```html
<!-- 桌面端表格 -->
<div style="overflow-x:auto;">
  <table style="width:100%; border-collapse:collapse; background:white; border-radius:[圆角]; overflow:hidden; box-shadow:0 2px 8px rgba(0,0,0,0.08);">
    <thead>
      <tr style="background:[主色]; color:white;">
        <th style="padding:12px 16px; text-align:left; font-weight:600;">[列标题]</th>
        <th style="padding:12px 16px; text-align:left; font-weight:600;">[列标题]</th>
        <th style="padding:12px 16px; text-align:left; font-weight:600;">[列标题]</th>
        <th style="padding:12px 16px; text-align:left; font-weight:600;">操作</th>
      </tr>
    </thead>
    <tbody>
      <tr style="border-bottom:1px solid #eee;">
        <td style="padding:12px 16px;">[数据]</td>
        <td style="padding:12px 16px;">[数据]</td>
        <td style="padding:12px 16px;">[数据]</td>
        <td style="padding:12px 16px;"><a href="#" style="color:[主色]; text-decoration:none; padding:8px; min-height:44px; display:inline-block;">编辑</a></td>
      </tr>
      <tr style="border-bottom:1px solid #eee; background:#fafafa;">
        <td style="padding:12px 16px;">[数据]</td>
        <td style="padding:12px 16px;">[数据]</td>
        <td style="padding:12px 16px;">[数据]</td>
        <td style="padding:12px 16px;"><a href="#" style="color:[主色]; text-decoration:none; padding:8px; min-height:44px; display:inline-block;">编辑</a></td>
      </tr>
    </tbody>
  </table>
</div>

<!-- 移动端列表（卡片式，替代表格） -->
<div>
  <div style="background:white; border-radius:[圆角]; padding:16px; margin-bottom:8px; box-shadow:0 1px 4px rgba(0,0,0,0.06);">
    <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:8px;">
      <span style="font-weight:600; color:[文字色];">[标题]</span>
      <span style="font-size:12px; color:#999;">[时间]</span>
    </div>
    <p style="margin:0; color:#666; font-size:14px;">[描述]</p>
    <div style="margin-top:12px; display:flex; gap:12px;">
      <button style="background:transparent; color:[主色]; border:1px solid [主色]; padding:8px 16px; border-radius:[圆角]; font-size:14px; min-height:44px;">编辑</button>
      <button style="background:transparent; color:#e74c3c; border:1px solid #e74c3c; padding:8px 16px; border-radius:[圆角]; font-size:14px; min-height:44px;">删除</button>
    </div>
  </div>
</div>
```

### 弹窗 / 对话框

```html
<!-- 遮罩层 + 弹窗 -->
<div style="position:fixed; top:0; left:0; right:0; bottom:0; background:rgba(0,0,0,0.5); display:flex; align-items:center; justify-content:center; z-index:1000;">
  <div style="background:white; border-radius:[圆角]; padding:24px; max-width:400px; width:90%; box-shadow:0 8px 32px rgba(0,0,0,0.2);">
    <h3 style="margin:0 0 12px; color:[文字色];">[弹窗标题]</h3>
    <p style="margin:0 0 24px; color:#666; font-size:14px; line-height:1.5;">[弹窗内容，说明要确认的事情]</p>
    <div style="display:flex; gap:12px; justify-content:flex-end;">
      <button style="background:white; color:#666; border:1px solid #ddd; padding:10px 20px; border-radius:[圆角]; font-size:14px; cursor:pointer; min-height:44px;">取消</button>
      <button style="background:[主色]; color:white; border:none; padding:10px 20px; border-radius:[圆角]; font-size:14px; cursor:pointer; min-height:44px;">确认</button>
    </div>
  </div>
</div>
```

### Tab 切换

```html
<div>
  <!-- Tab 头 -->
  <div style="display:flex; border-bottom:2px solid #eee;">
    <button style="padding:12px 24px; border:none; background:transparent; font-size:16px; cursor:pointer; color:[主色]; border-bottom:2px solid [主色]; margin-bottom:-2px; min-height:44px;">[标签1]</button>
    <button style="padding:12px 24px; border:none; background:transparent; font-size:16px; cursor:pointer; color:#999; min-height:44px;">[标签2]</button>
    <button style="padding:12px 24px; border:none; background:transparent; font-size:16px; cursor:pointer; color:#999; min-height:44px;">[标签3]</button>
  </div>
  <!-- Tab 内容 -->
  <div style="padding:20px 0;">
    <p style="color:#666;">[标签1 对应的内容]</p>
  </div>
</div>
```

### 轮播图 / Banner

```html
<div style="position:relative; overflow:hidden; border-radius:[圆角];">
  <!-- 轮播项 -->
  <div style="height:200px; background:linear-gradient(135deg, [主色], [辅色]); display:flex; align-items:center; justify-content:center; color:white;">
    <div style="text-align:center;">
      <h2 style="margin:0 0 8px; font-size:24px;">[轮播标题]</h2>
      <p style="margin:0 0 16px; font-size:14px; opacity:0.9;">[轮播描述]</p>
      <button style="background:white; color:[主色]; border:none; padding:10px 24px; border-radius:[圆角]; font-size:14px; cursor:pointer; min-height:44px;">了解更多</button>
    </div>
  </div>
  <!-- 指示器 -->
  <div style="position:absolute; bottom:12px; left:50%; transform:translateX(-50%); display:flex; gap:8px;">
    <span style="width:8px; height:8px; border-radius:50%; background:white;"></span>
    <span style="width:8px; height:8px; border-radius:50%; background:rgba(255,255,255,0.5);"></span>
    <span style="width:8px; height:8px; border-radius:50%; background:rgba(255,255,255,0.5);"></span>
  </div>
</div>
```

### 空状态占位

```html
<!-- 通用空状态 -->
<div style="text-align:center; padding:60px 20px; color:#999;">
  <div style="font-size:48px; margin-bottom:16px;">📭</div>
  <h3 style="margin:0 0 8px; color:#666; font-size:18px;">[空状态标题，如"还没有数据"]</h3>
  <p style="margin:0 0 20px; font-size:14px;">[引导文字，如"点击下方按钮创建第一个项目"]</p>
  <button style="background:[主色]; color:white; border:none; padding:12px 24px; border-radius:[圆角]; font-size:16px; cursor:pointer; min-height:44px;">[操作按钮]</button>
</div>

<!-- 搜索无结果 -->
<div style="text-align:center; padding:60px 20px; color:#999;">
  <div style="font-size:48px; margin-bottom:16px;">🔍</div>
  <h3 style="margin:0 0 8px; color:#666; font-size:18px;">没有找到相关内容</h3>
  <p style="margin:0; font-size:14px;">试试换个关键词搜索</p>
</div>
```

### 加载骨架屏

```html
<!-- 卡片骨架屏 -->
<div style="background:white; border-radius:[圆角]; padding:20px; box-shadow:0 2px 8px rgba(0,0,0,0.08); margin:12px;">
  <div style="display:flex; align-items:center; margin-bottom:16px;">
    <div style="width:40px; height:40px; border-radius:50%; background:#eee;"></div>
    <div style="margin-left:12px; flex:1;">
      <div style="height:14px; width:40%; background:#eee; border-radius:4px; margin-bottom:6px;"></div>
      <div style="height:10px; width:25%; background:#f5f5f5; border-radius:4px;"></div>
    </div>
  </div>
  <div style="height:14px; width:100%; background:#eee; border-radius:4px; margin-bottom:8px;"></div>
  <div style="height:14px; width:80%; background:#eee; border-radius:4px; margin-bottom:8px;"></div>
  <div style="height:14px; width:60%; background:#f5f5f5; border-radius:4px;"></div>
</div>

<!-- 列表骨架屏 -->
<div>
  <div style="display:flex; align-items:center; padding:16px; border-bottom:1px solid #f0f0f0;">
    <div style="width:48px; height:48px; border-radius:8px; background:#eee; flex-shrink:0;"></div>
    <div style="margin-left:12px; flex:1;">
      <div style="height:14px; width:50%; background:#eee; border-radius:4px; margin-bottom:8px;"></div>
      <div style="height:12px; width:70%; background:#f5f5f5; border-radius:4px;"></div>
    </div>
  </div>
  <div style="display:flex; align-items:center; padding:16px; border-bottom:1px solid #f0f0f0;">
    <div style="width:48px; height:48px; border-radius:8px; background:#eee; flex-shrink:0;"></div>
    <div style="margin-left:12px; flex:1;">
      <div style="height:14px; width:45%; background:#eee; border-radius:4px; margin-bottom:8px;"></div>
      <div style="height:12px; width:65%; background:#f5f5f5; border-radius:4px;"></div>
    </div>
  </div>
</div>
```

### 加载指示器

```html
<!-- 菊花转圈 -->
<div style="text-align:center; padding:40px;">
  <div style="display:inline-block; width:32px; height:32px; border:3px solid #eee; border-top-color:[主色]; border-radius:50%; animation:spin 0.8s linear infinite;"></div>
  <style>@keyframes spin { to { transform: rotate(360deg); } }</style>
  <p style="color:#999; font-size:14px; margin-top:12px;">加载中...</p>
</div>

<!-- 进度条 -->
<div style="background:#eee; border-radius:4px; height:4px; overflow:hidden;">
  <div style="background:[主色]; height:100%; width:60%; border-radius:4px; animation:progress 1.5s ease-in-out infinite;"></div>
  <style>@keyframes progress { 0% { width:0%; } 50% { width:70%; } 100% { width:100%; } }</style>
</div>
```

### 页脚

```html
<!-- 桌面端页脚 -->
<footer style="background:#333; color:#ccc; padding:40px 24px 20px;">
  <div style="display:flex; justify-content:space-between; flex-wrap:wrap; gap:24px; max-width:1200px; margin:0 auto;">
    <div>
      <h4 style="color:white; margin:0 0 12px;">[品牌名]</h4>
      <p style="margin:0; font-size:14px; line-height:1.6;">[一句话介绍]</p>
    </div>
    <div>
      <h4 style="color:white; margin:0 0 12px;">快速链接</h4>
      <a href="#" style="display:block; color:#ccc; text-decoration:none; font-size:14px; margin-bottom:8px;">首页</a>
      <a href="#" style="display:block; color:#ccc; text-decoration:none; font-size:14px; margin-bottom:8px;">功能</a>
      <a href="#" style="display:block; color:#ccc; text-decoration:none; font-size:14px;">关于</a>
    </div>
    <div>
      <h4 style="color:white; margin:0 0 12px;">联系方式</h4>
      <p style="margin:0; font-size:14px;">邮箱：example@email.com</p>
      <p style="margin:4px 0 0; font-size:14px;">电话：400-xxx-xxxx</p>
    </div>
  </div>
  <div style="text-align:center; margin-top:32px; padding-top:16px; border-top:1px solid #555; font-size:12px; color:#999;">
    © 2026 [品牌名]. All rights reserved.
  </div>
</footer>

<!-- 移动端页脚（简化版） -->
<footer style="background:#333; color:#ccc; padding:24px 16px; text-align:center;">
  <h4 style="color:white; margin:0 0 8px;">[品牌名]</h4>
  <p style="margin:0; font-size:12px; color:#999;">© 2026 [品牌名]. All rights reserved.</p>
</footer>
```

## 5. 使用要点

- 所有样式内联，不使用外部资源
- 占位文本用项目相关内容，不用 lorem ipsum
- 图片用纯色块 + 文字描述代替
- 颜色对比度符合可读性标准（WCAG AA，正文对比度 ≥ 4.5:1）
- **触控区域：** 移动端所有可点击元素不小于 44×44px（硬性规范）
- 移动端底部导航区域需要为固定定位的底部栏留出 padding-bottom（约 60px）
- 深色模式下注意文字与背景的对比度同样满足 WCAG AA
