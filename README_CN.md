# muse-skills（中文说明）

给 **Meta Muse** 用的实战 skills 和工作流模板合集。

这里没有玩具示例。每一个 skill 都在真实生产环境里每天被 AI agent 使用：配音视频生产、社交账号运营、晨间简报、收件箱分拣。哪个 skill 在现实里不好用了，就在这里修好或者删掉。

English: [README.md](./README.md)

## 有什么

```
skills/
├── tts-narration/      # 长配音的可靠做法：分段合成、逐段校验、
│                       # 语速控制、拼接
└── morning-briefing/   # Agent 工作流模板：把隔夜新闻压成一份
                        # 3 分钟能读完的晨间简报
```

每个 skill 是一个文件夹，里面有 `SKILL.md`（名称、描述、使用说明）——拷进 agent 的 skills 目录就能用。

## 安装

```bash
git clone https://github.com/kkone1238899/muse-skills.git
# 把想要的 skill 拷进 agent 的 skills 目录，例如
cp -r muse-skills/skills/tts-narration ~/.config/muse/skills/
```

先读该 skill 的 `SKILL.md`——里面写了需要的工具、输入和已知坑。

## 为什么做这个

市面上的 "awesome AI agent" 大多是链接收藏夹。这个仓库反着来：只收每天真实在用的少量 skills。欢迎贡献，但门槛是"我每天都在跑它"——见 [CONTRIBUTING.md](./CONTRIBUTING.md)。

## 路线图

- [ ] 社交媒体运营 skills（钩子写作、回复策略）
- [ ] 视频生产管线 skills（渲染、字幕、质检）
- [ ] 更多工作流模板：收件箱分拣、旅行规划、研究速览

## License

MIT，随便用。见 [LICENSE](./LICENSE)。
