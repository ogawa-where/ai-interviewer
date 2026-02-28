# AI Interviewer

NVIDIA [PersonaPlex-7B-v1](https://huggingface.co/nvidia/personaplex-7b-v1) を活用した、リアルタイム音声対話型の AI 面接官システムです。
ブラウザからマイクを通じて AI と全二重の音声会話を行い、面接練習などに利用できます。

## 主な特徴

- **リアルタイム全二重音声対話** — ユーザーと AI が同時に発話できる低遅延な音声パイプライン
- **ペルソナ・ボイス設定** — テキストプロンプトで AI の役割を指定し、13 種類のプリセット音声から選択可能
- **ブラウザ完結** — WebSocket + Opus コーデックによる音声ストリーミングで追加ソフト不要
- **会話録音** — AI 音声（L ch）とユーザー音声（R ch）をステレオ録音しダウンロード可能

## 必要環境

| 要件 | 詳細 |
|------|------|
| GPU | NVIDIA RTX 3090 Ti 以上（VRAM 24 GB 以上） |
| ランタイム | Docker + [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) |
| アカウント | HuggingFace アカウント + API トークン |

## セットアップ

### 1. リポジトリをクローン

```bash
git clone --recursive <repository-url>
cd ai-interviewer
```

> `--recursive` で PersonaPlex サブモジュールも取得されます。

### 2. HuggingFace でモデルライセンスに同意

[nvidia/personaplex-7b-v1](https://huggingface.co/nvidia/personaplex-7b-v1) にアクセスし、「Agree and access repository」をクリックしてください。

### 3. 環境変数を設定

```bash
cp .env.example .env
```

`.env` を編集し、[HuggingFace のトークン設定ページ](https://huggingface.co/settings/tokens)で取得したトークンを設定します。

```
HF_TOKEN=hf_your_token_here
```

### 4. 起動

```bash
docker compose up --build
```

初回起動時にモデル（約 14 GB）が自動ダウンロードされます。`./cache` にキャッシュされるため、2 回目以降は再ダウンロード不要です。

### 5. アクセス

ブラウザで以下にアクセスしてください（自己署名証明書を使用）。

```
https://localhost:8998
```

## アーキテクチャ

```
┌─────────────┐  WebSocket (Opus)   ┌──────────────────────┐
│   Browser   │◄───────────────────►│   Backend Server     │
│  (React)    │   wss://...:8998    │   (Python/aiohttp)   │
│             │                     │                      │
│ - Opus Enc  │                     │ - MimiModel (Enc/Dec)│
│ - WASM Dec  │                     │ - LMModel (7B LLM)   │
│ - Jitter Buf│                     │ - Voice Embeddings   │
└─────────────┘                     └──────────────────────┘
```

## 技術スタック

**バックエンド**: Python 3.12 / PyTorch / aiohttp / CUDA 12.4

**フロントエンド**: React 18 / TypeScript / Vite / Tailwind CSS + DaisyUI

## ライセンス

PersonaPlex-7B-v1: [NVIDIA Open Model License](https://huggingface.co/nvidia/personaplex-7b-v1)
