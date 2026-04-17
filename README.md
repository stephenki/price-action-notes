# 📚 价格行为学习笔记 (Price Action Notes)

> Al Brooks《Trading Price Action》系列书籍学习总结

本仓库包含对 Al Brooks 经典三部曲《Trading Price Action Trends/Ranges/Bars》的系统学习笔记，基于 Hermes Agent 自动提取和 AI 辅助整理。

---

## 📖 教材信息

- **作者**: Al Brooks
- **系列**: 
  - Book 1: Trends (趋势)
  - Book 2: Trading Ranges (交易区间)
  - Book 3: Trends (深入趋势，本仓库重点)
- **领域**: 价格行为交易学 (Price Action Trading)
- **适用市场**: 股票、期货、外汇、加密货币

---

## 📁 笔记结构

```
├── README.md
├── 2026-04-05.md  # Chapter 13 - Triangles and Flags
├── 2026-04-06.md  # Chapter 13 - 完善完成
├── 2026-04-07.md  # Chapter 14 - Parabolic and Overtrading
├── 2026-04-08.md  # Chapter 15 - Pullbacks
├── 2026-04-09.md  # Chapter 16 - Reversals
├── 2026-04-10.md  # Chapter 17 - Horizontal Lines
├── 2026-04-11.md  # Chapter 18 - Wedge Pullbacks
├── 2026-04-12.md  # Chapter 19 - Dueling Lines
├── 2026-04-21.md  # Chapter 20 - Gap Openings ⭐
├── 2026-04-22.md  # Chapter 21 - Day Trading Examples ⭐
├── 2026-04-23.md  # Chapter 22 - Multi-Timeframe Charts ⭐
├── 2026-04-24.md  # Chapter 23 - Options ⭐
├── 2026-04-25.md  # Chapter 24 - The Best Trades ⭐⭐⭐
└── 2026-04-26.md  # Chapter 25 - Trading Guidelines ⭐
```

> ⭐ 表示近期补充/完善的章节

---

## 🎯 核心主题索引

| 章节 | 主题 | 状态 | 字数 |
|------|------|------|------|
| Ch 13 | 持续形态（三角、旗形） | ✅ | ~10 KB |
| Ch 14 | 抛物线运动与过度交易 | ✅ | ~12 KB |
| Ch 15 | 回调类型与入场策略 | ✅ | ~19 KB |
| Ch 16 | 反转K线与反转结构 | ✅ | ~24 KB |
| Ch 17 | 摆动点与关键价格水平 | ✅ | ~11 KB |
| Ch 18 | 楔形与三推回调 | ✅ | ~15 KB |
| Ch 19 | 决斗线形态 | ✅ | ~52 KB |
| Ch 20 | 跳空开盘的交易意义 | ✅ | ~7 KB |
| Ch 21 | 日内交易实例详解 | ✅ | ~7 KB |
| Ch 22 | 多时间框架分析 | ✅ | ~8 KB |
| Ch 23 | 期权基础与价格行为 | ✅ | ~8 KB |
| Ch 24 | **全书总结：最佳交易** | ✅ | ~11 KB |
| Ch 25 | **交易指南：准则与哲学** | ✅ | ~7 KB |

---

## 🔧 学习方式

### 自动化流程

1. **每日学习** (6:00 AM cron)
   - 通过 Hermes Agent 自动执行
   - 从 EPUB 教材提取章节内容
   - AI 生成结构化学习笔记
   - 同步更新进度追踪

2. **EPUB 来源**
   - Book 3 (Trends): `Trading Price Action - Trends (Z-Library).epub`
   - 使用 Python + zipfile 解析，提取 HTML 内容
   - 通过关键词提取精华段落

3. **笔记格式**
   - 章节核心概念提取
   - 原文精华片段（25+ 段/章）
   - 交易应用要点
   - 关联章节索引

---

## 📊 进度追踪

- ✅ **Trends 部分**: Chapter 1-25 已完成学习
- ✅ **笔记整理**: Chapter 13-25 已完善
- ⏳ **进行中**: 持续优化笔记质量
- 📅 **计划**: 可选补充 Book 2 (Trading Ranges) 笔记

---

## 🔗 相关资源

- [Hermes Agent](https://github.com/NousResearch/hermes-agent) - 自动化学习工具
- [Price Action 交易社区](https://www.priceactiontrading.com/) - Brooks 方法讨论
- [Al Brooks 官方资源](https://brookspriceaction.com/)

---

## 📝 贡献

本仓库为个人学习笔记，供参考交流。所有内容源自 Al Brooks 著作，版权归原作者所有。

---

**最后更新**: 2026-04-17
**学习系统**: Hermes Agent v0.10.0 + EPUB 自动提取
**总笔记数**: 13 个章节
**总大小**: ~192 KB
