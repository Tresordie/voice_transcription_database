# voice_transcription_database

[VoiceTrans 语音转录应用](https://github.com/Tresordie/voice_transcription)的历史记录备份仓库（Gitee + GitHub 双推保存）。

应用内转录历史仅存于浏览器 IndexedDB，本仓库持久化其 JSON 导出文件，实现跨设备保留。

## 备份流程

### 方式一：应用自动备份（推荐，dev 模式可用）

1. `npm run dev` 启动应用 → 无需任何操作，应用运行期间按设定间隔（默认 5 分钟，可在历史页「git 备份」面板调整或关闭）自动推送：
   - 有新转录 → 自动写入 `data/history.json` + commit + push 到 GitHub + Gitee 双远端
   - 内容无变化 → 跳过本轮；上次推送失败 → 下一轮自动重试
2. 也可以随时在历史页点「**备份并推送**」立即执行，或点「推送」手动触发
3. 页面「git 备份」状态区实时显示仓库路径、`data/history.json` 更新时间、最近提交与每个远端的推送结果

> 自动备份仅在本机 dev 模式且应用运行期间生效；备份仓库路径不在此处时，应用侧可用环境变量 `BACKUP_REPO_PATH` 指定。

### 方式二：手动备份（应用未用 dev 模式启动时）

1. 「历史记录」页点「导出全部」，得到 `历史备份-YYYYMMDD-HHMMSS.json`（备份服务未连接时自动退回浏览器下载）
2. 将文件放入本仓库 `data/` 目录，重命名为 `history.json`
3. 在应用历史页点「推送」，或手动执行：

```bash
git add data/history.json
git commit -m "backup: 历史记录备份 2026-11-23"
git push   # 一次 push 同时推 GitHub + Gitee 双远端
```

## 恢复流程

换设备或清浏览器数据后：

1. 打开 VoiceTrans 应用 → 「历史记录」页 → 点击「导入」
2. 选择本仓库 `data/history.json`，应用按 id 合并去重回灌，不覆盖已有记录

## 数据格式

```json
{
  "version": 1,
  "exportedAt": "2026-11-23T12:00:00.000Z",
  "count": 42,
  "items": [
    { "id": "m1abc2-xyz", "createdAt": 1763875200000, "source": "live", "engine": "本地模型", "durationSec": 12.3, "text": "……" }
  ]
}
```

> 仅备份转录历史；应用设置（含 API Key）存于浏览器 localStorage，属敏感信息，不纳入备份。
