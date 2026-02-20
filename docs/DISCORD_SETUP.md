# OpenClaw × Discord セットアップ

Discord で OpenClaw を使うための手順。Bot トークン取得までをまとめる。

## 1. Discord で Bot を作る

1. **Developer Portal** を開く  
   https://discord.com/developers/applications
2. **Applications** → **New Application** でアプリを作成（名前は任意）
3. 左メニュー **Bot** → **Add Bot**
4. **Reset Token** でトークンを表示し、**Copy** でコピー（このあと再表示できないので控えておく）

## 2. Privileged Intents を有効にする

**Bot** 画面の **Privileged Gateway Intents** で以下を ON にする:

- **Message Content Intent**（必須・メッセージを読むため）
- **Server Members Intent**（推奨・メンバー/ロール判定に使う）

変更後は **Save Changes**。

## 2.5. 「Integration required code grant」が出る場合

このエラーは、Bot の **Requires OAuth2 Code Grant** が ON のときに、通常の招待 URL（code grant なし）で追加しようとすると出る。**Bot だけを招待する場合は OFF にする。**

1. 左メニュー **Bot** を開く
2. **Authorization Flow** の **Requires OAuth2 Code Grant** のスイッチを **OFF** にする
3. **Save Changes**

これで通常の招待 URL で追加できる。逆に、複数スコープで OAuth2 フローを使う場合は ON のままにして、**OAuth2** で **Redirects** を 1 件以上追加しておく。

## 3. Bot をサーバーに招待する

**OpenClaw 用の招待 URL（アプリ ID 1473373126304989387）:**

```
https://discord.com/api/oauth2/authorize?client_id=1473373126304989387&permissions=277025508352&scope=bot%20applications.commands
```

- 上記 URL をブラウザで開く
- 招待先のサーバーを選んで **Authorize** → 完了
- 権限: View Channels, Send Messages, Read Message History, Embed Links, Attach Files, Add Reactions, Use Slash Commands

## 4. OpenClaw にトークンを渡す

どちらか一方でよい。

**A. 環境変数（推奨・トークンをファイルに書かない）**

- Gateway を起動する環境で `DISCORD_BOT_TOKEN=あなたのBotトークン` を設定
- LaunchAgent で動かしている場合は、plist の `EnvironmentVariables` に `DISCORD_BOT_TOKEN` を追加するか、`openclaw gateway install` をやり直して config の `env.vars` に `DISCORD_BOT_TOKEN` を入れておく

**B. 設定ファイル**

- `~/.openclaw/openclaw.json` の `channels.discord.token` に Bot トークンを書く  
- 現在は `channels.discord` を有効にしてあり、`token` は空。ここに貼るか、上記のとおり環境変数で渡す。

## 5. Gateway を再起動する

```bash
openclaw gateway restart
```

## 6. DM で使う場合（ペアリング）

DM はデフォルトで **ペアリングモード**。初回は以下でコードを確認してから承認する:

```bash
openclaw pairing list discord
openclaw pairing approve discord <表示されたコード>
```

コードは 1 時間で失効する。

## 7. サーバー（ギルド）で使う場合

- デフォルトは **allowlist**。使いたいサーバーを `channels.discord.guilds` に追加する必要がある。
- サーバー ID は Discord の **設定 → 詳細 → 開発者モード** を ON にしたあと、サーバー名を右クリック → **ID をコピー** で取得。
- 設定例は [Discord - OpenClaw](https://docs.openclaw.ai/channels/discord) の「Guild policy」を参照。

## トラブルシューティング

- メッセージが読めない → Message Content Intent が ON か確認し、Gateway を再起動
- 「Not Authorized」→ OpenClaw 側の許可（ペアリング承認 or guild allowlist）を確認
- ログ確認: `openclaw channels status --probe` / `openclaw logs --follow`

公式: [Discord - OpenClaw](https://docs.openclaw.ai/channels/discord)
