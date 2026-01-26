# AI Interviewer（開発中...）

NVIDIA PersonaPlex-7B-v1を活用したAI面接官システム

## セットアップ

### 1. クローン

```bash
git clone --recursive <repository-url>
cd ai-interviewer
```

### 2. HuggingFaceでライセンスに同意

https://huggingface.co/nvidia/personaplex-7b-v1 で「Agree and access repository」をクリック

### 3. 環境変数を設定

```bash
cp .env.example .env
# .env にHF_TOKENを設定
```

### 4. 起動

```bash
docker compose up --build
```

### 5. アクセス

https://localhost:8998

## 必要環境

- NVIDIA GPU (RTX 3090 Ti以上、24GB VRAM)
- Docker + NVIDIA Container Toolkit
- HuggingFace Token

## ライセンス

PersonaPlex-7B-v1: NVIDIA Open Model License
