# 股票新闻情感分析可视化系统设计文档

## 1. 项目概述

本项目是一个基于 FastAPI 和 Vue.js 的全栈网站，支持电脑和手机浏览器自适应。主要功能是分析近七天的个股新闻情感，并以现代化的可视化方式呈现。

### 1.1 核心功能

- 支持通过股票名称搜索，自动转换为股票代码
- 分析并展示近 7 天的新闻情感数据
- 多维度情感分析（整体情绪、时间趋势、主题分析等）
- 丰富的数据可视化展示
- 响应式设计，支持多端访问

## 2. 技术栈选择

### 2.1 前端技术栈

- Vue 3 (Options API)
- Element Plus (UI 框架)
- ECharts (数据可视化)
- Vuex (状态管理)
- Axios (HTTP 客户端)
- TailwindCSS (样式框架)

### 2.2 后端技术栈

- FastAPI (Web 框架)
- Poetry (依赖管理)
- google.genai (Gemini API 调用)
- akshare (股票数据获取)
- pathlib (跨平台路径处理)
- asyncio (异步处理)

### 2.3 缓存机制

- 新闻数据缓存：避免重复爬取，缓存期 1 天
- 情感分析缓存：减少 API 调用，缓存期 1 天
- 缓存存储：本地文件系统 (JSON 格式)
- 缓存策略：基于新闻内容 hash 的缓存键

## 3. 系统架构

### 3.1 目录结构

```
project/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── SearchBar.vue
│   │   │   ├── SentimentDashboard/
│   │   │   │   ├── MainGauge.vue        # 主情感仪表盘
│   │   │   │   ├── ConfidenceChart.vue  # 信心指数图表
│   │   │   │   ├── RiskReward.vue       # 风险收益分析
│   │   │   │   └── MarketMood.vue       # 市场情绪指标
│   │   │   ├── TrendAnalysis/
│   │   │   │   ├── TimeSeries.vue       # 时间序列图表
│   │   │   │   ├── EventMarkers.vue     # 关键事件标记
│   │   │   │   └── PredictionZone.vue   # 预测区域
│   │   │   ├── TopicAnalysis/
│   │   │   │   ├── RadarChart.vue       # 主题雷达图
│   │   │   │   └── TopicBreakdown.vue   # 主题详情
│   │   │   ├── SourceAnalysis/
│   │   │   │   ├── CredibilityChart.vue # 可信度分析
│   │   │   │   └── MediaBreakdown.vue   # 媒体来源分析
│   │   │   └── NewsList/
│   │   │       ├── NewsCard.vue         # 新闻卡片
│   │   │       └── NewsFilter.vue       # 新闻筛选器
│   │   ├── views/
│   │   │   ├── Home.vue
│   │   │   └── Analysis.vue
│   │   ├── api/
│   │   │   ├── stock.js
│   │   │   └── news.js
│   │   ├── store/
│   │   │   └── index.js
│   │   └── styles/
│   │       └── theme.css
│   └── public/
├── backend/
│   ├── api/
│   │   └── routes.py          # API路由
│   ├── core/
│   │   ├── news_crawler.py    # 新闻爬虫
│   │   └── sentiment_analyzer.py # 情感分析
│   ├── utils/
│   │   ├── config.py          # 配置管理
│   │   └── gemini_utils.py    # LLM工具
│   └── data/
│       ├── news_cache/        # 新闻缓存
│       └── sentiment_cache/   # 情感分析缓存
└── docs/
```

## 4. 后端 API 设计

### 4.1 主要接口

#### 4.1.1 股票分析接口

```python
@router.get("/api/stock-analysis/{stock_code}")
async def analyze_stock(
    stock_code: str,
    days: int = 7,
    max_news: int = 20,
    sentiment_news: int = 10
)
```

返回数据结构：

```json
{
    "overall_sentiment": {
        "score": 0.0,          # -1到1
        "confidence_index": 0,  # 0-100
        "market_expectation_strength": 0,  # -100到100
        "investor_sentiment": 0,  # 0-100
        "label": "string",
        "key_factors": [...]
    },
    "time_analysis": {
        "trend": [...],
        "trend_prediction": {...}
    },
    "topic_analysis": {
        "company_operation": {...},
        "financial_performance": {...},
        "market_competition": {...},
        "product_technology": {...},
        "industry_policy": {...},
        "capital_market": {...}
    },
    "source_analysis": {...},
    "impact_analysis": {...},
    "risk_reward_analysis": {...},
    "technical_analysis": {...},
    "fundamental_analysis": {...}
}
```

## 5. 前端设计

### 5.1 视觉主题

#### 5.1.1 配色方案

- 主背景：深邃星空渐变 (#0B1026 到 #2A2F4E)
- 主要前景：电子蓝 (#00F0FF)
- 辅助色系：
  - 霓虹紫 (#7B42FF)
  - 赛博粉 (#FF2E6C)
  - 量子绿 (#39FF14)
- 情感色系：
  - 积极：量子绿 (#39FF14)
  - 消极：电浆红 (#FF3860)
  - 中性：离子黄 (#FFD700)
- 透明元素：
  - 卡片背景：rgba(255, 255, 255, 0.07)
  - 浮层背景：rgba(11, 16, 38, 0.85)
  - 高亮边框：rgba(0, 240, 255, 0.3)

#### 5.1.2 视觉效果

- 背景：
  - 动态星空粒子效果
  - 渐变光晕流动
  - 响应鼠标移动的视差效果
- 毛玻璃效果：
  - 背景模糊：blur(20px)
  - 亮度提升：brightness(1.1)
  - 对比度：contrast(0.9)
- 光效：
  - 霓虹边框发光效果
  - 悬浮状态光晕扩散
  - 数据流动光效

#### 5.1.3 动效设计

- 转场动画：
  - 页面切换：3D 翻转效果
  - 组件加载：渐现上浮
  - 数据更新：波纹扩散
- 交互反馈：
  - 点击涟漪效果
  - 悬浮光晕扩散
  - 选中状态能量流动

### 5.2 主要组件设计

#### 5.2.1 情感分析仪表盘 (SentimentDashboard)

1. 主情感仪表盘

   - 设计：
     - 3D 环形仪表盘
     - 内部粒子流动效果
     - 动态光晕边框
   - 交互：
     - 点击展开详细分析
     - 悬浮显示历史数据
     - 双击重置视图
   - 动效：
     - 数值变化时能量波动
     - 颜色渐变过渡
     - 刻度动态伸缩

2. 信心指数面板

   - 设计：
     - 垂直分段进度条
     - 立体光效渲染
     - 实时数值浮动显示
   - 交互：
     - 点击查看指数构成
     - 拖动查看历史数据
     - 双指缩放时间范围
   - 动效：
     - 能量流动动画
     - 数值跳动特效
     - 状态切换转场

3. 市场情绪指标
   - 设计：
     - 环形情绪光谱
     - 多层次立体显示
     - 实时脉冲效果
   - 交互：
     - 点击展开详情面板
     - 滑动切换时间维度
     - 手势缩放查看趋势
   - 动效：
     - 情绪切换波纹
     - 数据更新脉冲
     - 选中状态光环

#### 5.2.2 趋势分析图表 (TrendAnalysis)

1. 主时间序列图

   - 设计：
     - 3D 视角曲线图
     - 渐变填充区域
     - 浮动数据标签
   - 交互：
     - 拖拽平移时间轴
     - 双指缩放时间范围
     - 点击显示详细数据
   - 动效：
     - 曲线绘制动画
     - 数据点呼吸效果
     - 区域流动渐变

2. 关键事件标记
   - 设计：
     - 悬浮事件卡片
     - 重要性等级标识
     - 关联新闻预览
   - 交互：
     - 点击展开事件详情
     - 上滑查看相关新闻
     - 左右滑动切换事件
   - 动效：
     - 卡片展开动画
     - 内容淡入效果
     - 滑动转场特效

#### 5.2.3 新闻分析系统 (NewsAnalysis)

1. 新闻卡片设计

   - 视觉：
     - 毛玻璃背景效果
     - 情感标签光晕
     - 立体阴影层次
   - 布局：
     - 网格瀑布流布局
     - 自适应卡片大小
     - 重要性视觉区分
   - 内容：
     - 标题与摘要
     - 情感得分标签
     - 时间与来源信息
     - 可信度指标
     - 相关性标记

2. 新闻交互功能

   - 基础交互：
     - 点击展开全文
     - 上滑加载更多
     - 双击收藏
   - 高级功能：
     - 相关新闻推荐
     - 时间线定位
     - 情感趋势关联
     - 来源可信度分析
   - 筛选功能：
     - 时间范围选择
     - 情感倾向筛选
     - 来源类型筛选
     - 相关度排序
     - 重要性排序

3. 新闻详情面板
   - 内容展示：
     - 完整新闻内容
     - 情感分析详情
     - 关键词提取
     - 相关股票信息
   - 交互功能：
     - 文本高亮显示
     - 关键词跳转
     - 相关新闻推荐
     - 分享功能
   - 数据联动：
     - 时间轴定位
     - 情感趋势关联
     - 主题分析关联

### 5.3 高级交互功能

1. 全局数据联动

   - 时间维度联动：
     - 图表同步缩放
     - 数据同步更新
     - 视图同步切换
   - 主题维度联动：
     - 相关数据高亮
     - 关联信息提示
     - 上下文切换

2. 智能交互助手

   - 功能引导：
     - 首次使用向导
     - 功能提示气泡
     - 操作示例动画
   - 数据解读：
     - 关键信息提示
     - 异常数据标记
     - 趋势变化提醒
   - 个性化推荐：
     - 相关股票推荐
     - 重要新闻提醒
     - 自定义监控

3. 手势操作支持
   - 基础手势：
     - 左右滑动切换
     - 上下滑动滚动
     - 双指缩放
   - 高级手势：
     - 长按查看详情
     - 双击重置视图
     - 三指切换主题

### 5.4 响应式设计优化

1. 桌面端（>1200px）

   - 多列网格布局
   - 悬浮预览功能
   - 快捷键支持
   - 高级图表功能

2. 平板端（768px-1200px）

   - 双列自适应布局
   - 手势操作优化
   - 简化图表显示
   - 触摸友好交互

3. 移动端（<768px）
   - 单列流式布局
   - 关键信息优先
   - 简化交互方式
   - 性能优化

### 5.5 性能优化

1. 视觉效果优化

   - 动效性能分级
   - 低端设备降级
   - 按需加载特效
   - GPU 加速渲染

2. 交互响应优化
   - 预加载数据
   - 延迟加载组件
   - 事件节流
   - 虚拟滚动

## 6. 性能优化

### 6.1 前端优化

- 组件懒加载
- 图表数据分片加载
- 本地数据缓存
- 防抖和节流处理

### 6.2 后端优化

- 新闻数据缓存
- 情感分析结果缓存
- 异步处理
- 数据压缩

## 7. 部署方案

### 7.1 开发环境

- 前端: Vite 开发服务器
- 后端: Uvicorn 开发服务器
- 数据: 本地文件系统

### 7.2 生产环境

- 前端: Nginx
- 后端: Gunicorn + Uvicorn
- 缓存: Redis (可选)
- 监控: Prometheus + Grafana (可选)
