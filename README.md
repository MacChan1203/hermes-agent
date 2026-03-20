# 🧠 Hermes Agent（拡張版 CLI AI エージェント）

このプロジェクトは、自然言語でターミナル操作を行える **CLI型AIエージェント** です。  
従来のシェル操作に加えて、状態（CWD・履歴）を保持しながらコマンドを実行できます。

---

## ✨ 特徴

### 🗣️ 自然言語で操作可能
```bash
今どこ？
フォルダ一覧見せて
親フォルダに戻って
gitの状態を表示して
```

---

### 📂 CWD（作業ディレクトリ）を保持
- `cd` は実行ではなく状態として管理
- セッションをまたいでも維持（`.hermes_cwd`）

---

### 🔁 前回コマンドの再実行
```bash
前回のコマンドもう一回
```

- コマンドだけでなく **実行時のディレクトリも復元**
- `.hermes_last_command`
- `.hermes_last_command_cwd`

---

### 🛠️ コマンドは直接実行（LLMを介さない）
- `terminal` ツールを直接呼び出し
- 不要な推論を排除し、安定動作

---

### ⚠️ 危険コマンド防止
以下のようなコマンドは実行を拒否します：

- `rm -rf /`
- `shutdown`
- `mkfs`
- fork bomb など

---

## 🚀 使い方

### 起動
```bash
OPENAI_API_KEY=ollama python run_agent.py --query "今どこ？"
```

---

### 例

```bash
# ディレクトリ移動
python run_agent.py --query "cd hermes-agent を実行して"

# Git確認
python run_agent.py --query "git status を表示して"

# 再実行
python run_agent.py --query "前回のコマンドもう一回"
```

---

## 📁 状態ファイル

| ファイル | 内容 |
|--------|------|
| `.hermes_cwd` | 現在の作業ディレクトリ |
| `.hermes_last_command` | 前回実行したコマンド |
| `.hermes_last_command_cwd` | 前回実行時のディレクトリ |

---

## 🧩 アーキテクチャ概要

```text
自然言語
   ↓
正規化（natural_command_map）
   ↓
コマンド抽出（regex）
   ↓
状態管理（cwd / last_command）
   ↓
terminal実行
```

---

## 🧠 設計思想

- LLMは「解釈」のみ
- 実行は常に deterministic（確定的）
- 状態は明示的に保存・復元
- CLIとAIの融合

---

## 🤝 開発について

このプロジェクトは **ChatGPTと共同で開発** されました。

- 設計支援
- デバッグ支援
- 機能拡張（CWD保持 / 再実行機能など）

を通じて構築されています。

---

## 📌 今後の拡張案

- コマンド履歴一覧
- 複数コマンドの分解実行
- 確認付き実行モード（安全性向上）
- GUI連携

---

## 🪪 ライセンス

自由に改変・利用可能（用途に応じて追記してください）

---

## 👤 作者

Macちゃん  
（ChatGPTとの共同開発）
