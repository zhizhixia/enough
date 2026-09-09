# 演示 GIF 录制指南（30–60 秒）

目标：展示 Enough 的当前行为——先按风险分级；只有真正存在“采用还是自建”取舍时，才做外部复用决策。

## 建议场景（推荐第一条）

1. 在支持 Enough 的客户端中新建会话。
2. 输入：`使用 $enough 推进：我想做一个 Windows 本地 RSS 阅读器，支持多源订阅、关键词过滤、定时刷新和一键安装。`
3. 展示输出先把它判为真实产品选型，再给出候选表、证据等级（E1/E2/E3）、“直接使用 RSS Guard”的推荐和下一步。
4. 收尾画面展示 README 的 FAST / NORMAL / DEEP 表，说明小修复不会被强制搜索或走完整流程。

> 不同客户端的自动触发能力不同。若没有自动触发，使用 `$enough` 或该客户端等价的显式 Skill 调用语法；独立安装不会自动安装 Hermes 的全局路由规则。

## 录制工具（Windows）

- 方案 A：ScreenToGif（免费开源，https://www.screentogif.com）——录制后直接导出 GIF。
- 方案 B：OBS Studio 录屏 + ffmpeg 转 GIF。

## 转 GIF 命令（如用 ffmpeg）

```bash
ffmpeg -i demo.mp4 -vf "fps=10,scale=1280:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" -loop 0 demo.gif
```

## 完成后

1. 将 GIF 放到仓库的 `docs/` 或 `assets/`，并使用相对链接。
2. 在 README 顶部引用，例如：

```markdown
![Enough 演示](docs/demo.gif)
```

3. 在发布前检查 GIF 不含密钥、个人数据、未授权项目内容或客户端敏感界面；是否提交、推送或发布仍需单独授权。
