---
tags:
  - tools
  - configuration
  - local-setup
aliases:
  - Tool Configuration
  - Local Notes
created: 2026-02-20
modified: 2026-02-21
---

# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

### OpenClaw × Discord

- セットアップ手順: [DISCORD_SETUP.md](./docs/DISCORD_SETUP.md)
- Bot トークンは `~/.openclaw/openclaw.json` の `channels.discord.token` か環境変数 `DISCORD_BOT_TOKEN` で指定。Gateway 再起動後に有効。

### Obsidian

**設定日：** 2026-02-21
- ワークスペース全体をObsidian vaultとして使用
- パス：`/Users/lost_and_found/.openclaw/workspace`
- メリット：リンク機能、グラフビュー、タグ・バックリンクでの情報整理

### カレンダー（gcalcli）

**セットアップ完了：** 2026-02-20
- Google Calendar API 連携済み
- デフォルトカレンダー：`Main`

**絶対ルール（2026-02-20 失敗から学んだ教訓）：**
1. **必ず `session_status` で現在日時を確認してから作業**
2. 相対的な日付（来週/来月/○日後）は `date` コマンドで検証
3. **予定追加前に必ず確認を取る：「○月○日（○）○時で合っていますか？」**
4. 登録後に `gcalcli agenda <日付>` で確認

**コマンドテンプレート：**
```bash
# 予定追加
gcalcli add --calendar "Main" --title "タイトル" --where "場所" --when "YYYY-MM-DD HH:MM" --duration 120 --noprompt

# 予定確認
gcalcli agenda YYYY-MM-DD

# 予定削除
gcalcli delete --iamaexpert "タイトル"
```

## What Goes Here

Things like:

- Camera names and locations
- SSH hosts and aliases
- Preferred voices for TTS
- Speaker/room names
- Device nicknames
- Anything environment-specific

## Examples

```markdown
### Cameras

- living-room → Main area, 180° wide angle
- front-door → Entrance, motion-triggered

### SSH

- home-server → 192.168.1.100, user: admin

### TTS

- Preferred voice: "Nova" (warm, slightly British)
- Default speaker: Kitchen HomePod
```

## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.
