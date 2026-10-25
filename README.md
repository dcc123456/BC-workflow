# BC-workflow

Browser Copilot 的共享工作流库。工作流由浏览器扩展里的「发布」按钮提交 PR，维护者手动合并后即出现在扩展的「更多 → 工作流广场」。

本仓库不运行任何服务，也不依赖 GitHub Actions：扩展直接读取 GitHub API 与 `raw.githubusercontent.com`，点赞与评论落在合并后的 PR 讨论区。

## 目录约定

```
workflows/<slug>/
  meta.json      工作流元数据（卡片展示）
  workflow.json  扩展导出的完整工作流记录
  assets/1.webp  可选封面图，最多 3 张，每张 ≤ 1 MB
```

- `slug`：小写 ASCII，匹配 `^[a-z0-9]+(-[a-z0-9]+)*$`，最长 64 字符，由扩展生成（可读词干 + 6 位内容哈希），**发布后不再修改**。
- `meta.json`（`schema` 固定为 `1`）：

| 字段         | 类型     | 说明                                                      |
| ------------ | -------- | --------------------------------------------------------- |
| `slug`       | string   | 与目录名一致                                              |
| `title`      | string   | ≤ 120 字符                                                |
| `description`| string   | ≤ 2000 字符                                               |
| `category`   | string   | `ecommerce` / `content` / `data` / `office` / `social` / `other` |
| `tags`       | string[] | 最多 8 个，每个 ≤ 24 字符                                 |
| `author`     | string   | 发起 PR 的 GitHub 登录名                                  |
| `version`    | number   | 从 1 开始，每次被合并的更新 +1                            |
| `hash`       | string   | `workflow.json` 的 SHA-256 十六进制摘要                   |
| `images`     | string[] | 仓库内相对路径，如 `assets/1.webp`                        |
| `videoUrl`   | string?  | 可选外链；视频文件不入库                                  |
| `createdAt`  | string   | ISO 时间                                                  |
| `updatedAt`  | string   | ISO 时间                                                  |

- `workflow.json` 就是扩展本地工作流记录的 JSON（结构见扩展仓库 `src/lib/workflow/types.ts`），`meta.json.hash` 必须等于它的 SHA-256。

## 如何发布

1. 扩展 → 工作流 → 任意工作流卡片点「发布」。
2. 首次使用需连接 GitHub：在弹窗里打开 token 创建页，创建 **classic** token 并只勾选 `public_repo`，粘贴验证。浏览广场不需要 token。
3. 填写标题、描述、分类、标签，可选封面图与视频链接，提交。
4. 扩展会自动 fork 本仓库、建 `submit/<slug>-<时间戳>` 分支、提交 `workflows/<slug>/` 并开 PR。
5. 维护者审核并合并。合并后的内容即对所有人可见（客户端缓存 30 分钟）。

## 审核规则

- 不得包含 token、密码、cookie、身份证件或个人数据；扩展发布时会剥离运行日志与截图，合并前请再确认一次。
- `meta.json` 与实际文件一致：`hash` 对得上、图片路径存在、版本号递增。
- 描述与标签准确，分类正确；不接受与工作流无关的素材。
- 视频只允许外链。
- 导入方默认拿到「触发关闭」的副本，工作流不会自动运行；请勿发布依赖隐藏账号状态才能工作的流程。

## 更新与下架

- **更新**：保持 `slug` 不变，`version` +1，替换 `workflow.json` 并更新 `hash`、`updatedAt`、`images`。已安装的用户会看到「可更新」。
- **下架**：开一个删除该目录的 PR 并说明原因；已安装的本地副本会保留，只是不再提示更新。

## 点赞与评论

点赞就是合并后 PR 上的 👍 reaction，评论就是该 PR 的讨论。扩展内展开卡片即可操作；没有连接 GitHub 时会给出去 GitHub 的链接。

## English

BC-workflow is the shared workflow library for Browser Copilot. Workflows arrive as pull requests opened from the extension's **Publish** action; the maintainer merges them by hand. There is no server and no CI: the extension reads the GitHub API and raw files, and likes/comments are reactions and comments on the merged PR. See the directory layout and `meta.json` fields above. Do not commit secrets, personal data, or video files; images are WebP, at most three, ≤ 1 MB each.
