# Contributing

## 最快的路径：用扩展发布

不需要手动建文件。在 Browser Copilot 里点「发布」，填写元数据，扩展会 fork、建分支、提交 `workflows/<slug>/` 并开 PR。维护者合并后即上线。

## 手动提交 PR

如果你要改文档、修一个已合并的工作流，或修 `meta.json` 里写错的信息：

1. fork 本仓库并新建分支。
2. 一个 PR 只做一件事：新增一个 `workflows/<slug>/`，或修改一个已有目录。
3. 只能改自己发起的目录；想改别人的工作流，请在 PR 里说明并征得作者同意。

## 文件要求

```
workflows/<slug>/
  meta.json
  workflow.json
  assets/1.webp   # 0–3 张
```

- `meta.json` 字段与校验规则见 README；`schema` 必须是 `1`。
- `meta.json.hash` 必须等于 `workflow.json` 的 SHA-256（`shasum -a 256 workflow.json`）。
- 新增：`version` 从 `1` 开始。更新：`slug` 与目录名不变，`version` 加一。
- 图片：WebP，单张 ≤ 1 MB，最多 3 张；不接受视频文件，只接受 `videoUrl` 外链。
- 不要提交 `node_modules`、压缩包、日志、截图证据等无关文件。

## 安全

- 不得包含任何形式的凭据（token、密码、API key、cookie）与个人数据。
- 工作流必须是手动触发的；不要提交依赖特定账号登录态才能跑通的流程。
- 维护者合并前会抽查 `workflow.json` 中是否残留 `password` / `token` / `apiKey` 一类字段。

## 维护者合并清单

1. PR 只包含 `workflows/<slug>/`（或文档）改动。
2. `meta.json` 可解析，`slug`/`title`/`category`/`version`/`hash` 合法且与文件一致。
3. 图片存在、体积合规；无凭据或隐私数据。
4. 更新类 PR 的 `version` 确实递增；删除类 PR 有说明。
5. 合并后用扩展的「工作流广场」刷新确认卡片正常显示。
