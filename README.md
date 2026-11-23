# voice_transcription_database

[VoiceTrans 语音转录应用](https://github.com/Tresordie/voice_transcription)的历史记录备份仓库（Gitee + GitHub 双推保存）。

应用内转录历史仅存于浏览器 IndexedDB，本仓库持久化其 JSON 导出文件，实现跨设备保留。

## 备份流程

1. 打开 VoiceTrans 应用 → 「历史记录」页 → 点击「导出全部」，得到 `历史备份-YYYYMMDD-HHMMSS.json`
2. 将文件放入本仓库 `data/` 目录，重命名为 `history.json`（覆盖旧备份；git 历史保留每一版）
3. 提交并推送（一次 push 同时推 GitHub + Gitee 双远端）：

```bash
git add data/history.json
git commit -m "backup: 历史记录备份 2026-11-23"
git push
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
