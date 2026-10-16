# Gemini Nano Banana 2.1 API (`gemini-nano-banana-2.1`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0137 per image** on the APIMart route — `https://api.apimart.ai/v1` の OpenAI 互換エンドポイント1つだけ、月額プランは不要です。*(2026-10-12 時点)*

**[APIキー](https://go.apimart.ai/k-3bc92b)** · **[料金](https://go.apimart.ai/k-8e44ce)** · **[モデルページ](https://go.apimart.ai/k-818e99)** · [⚡ 60秒クイックスタート](#quickstart)

**チームが APIMart 経由で Gemini Nano Banana 2.1（`gemini-nano-banana-2.1`）を使う理由**

- **キー1つでカタログ全体。**同じ `https://api.apimart.ai/v1` の base URL と `Authorization` ヘッダーで Gemini Nano Banana 2.1（`gemini-nano-banana-2.1`）と画像・動画・言語モデルなど 300+ のモデルにアクセスできます。切り替えるのは `model` フィールドだけで、クライアント側の変更は不要です。
- **最低 $1 から、従量課金。**サブスクリプションも、事前に規模を見積もるプリペイドプランもありません。$1 からチャージして、呼び出した分だけ使えます。
- **課金内容はレスポンスで返ります。**完了したタスクはすべて請求額（`cost` / `credits_cost`）を報告し、`failed` になったタスクは返金されます。
- **非同期前提の設計。**送信して `task_id` を受け取り、`GET /v1/tasks/{id}` をポーリングするだけ。バッチ処理もリトライも特別な統合ではなく、普通のキュー処理です。

<!-- /conv-kit:v1 -->

**別名:** ナノバナナ · なのばなな

Gemini Nano Banana 2.1 is Google's image model on APIMart: text-to-image and reference-image editing on the official or Ext route with 1K / 2K / 4K output.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `gemini-nano-banana-2.1` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-818e99) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |

Token rates (effective): `text_input` $1.2/M, `image_input` $1.2/M, `text_output` $6/M, `image_output` $24/M.
<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-8e44ce) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-818e99) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"gemini-nano-banana-2.1","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "gemini-nano-banana-2.1", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Gemini Nano Banana 2.1 のモデル id は何ですか？**

`gemini-nano-banana-2.1` です。上のルート表を参照してください。

**課金方法は？**

**from $0.0137 per image** on the APIMart route。完了したタスクは `cost` フィールドに正確な金額を報告し、失敗したタスクは返金されます。

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-3bc92b) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [gemini-nano-banana-2-1-prompts-ja](https://github.com/uey78jd5/gemini-nano-banana-2-1-prompts-ja)
- **Same model, other languages** — 9 more language builds: 中文 [gemini-nano-banana-2-1-api-cn](https://github.com/v5d5suw9e8/gemini-nano-banana-2-1-api-cn) · Deutsch [gemini-nano-banana-2-1-api-de](https://github.com/t8f-w3/gemini-nano-banana-2-1-api-de) · Español [gemini-nano-banana-2-1-api-es](https://github.com/mp1wvenc2s08n8/gemini-nano-banana-2-1-api-es) · Français [gemini-nano-banana-2-1-api-fr](https://github.com/tz0cbrs/gemini-nano-banana-2-1-api-fr) · Relay/gateway [gemini-nano-banana-2-1-api-gateway](https://github.com/fpe7tfhs07q/gemini-nano-banana-2-1-api-gateway) · Bahasa [gemini-nano-banana-2-1-api-id](https://github.com/a-kiecshv/gemini-nano-banana-2-1-api-id) · 한국어 [gemini-nano-banana-2-1-api-ko](https://github.com/xou-fm9md3t/gemini-nano-banana-2-1-api-ko) · Português [gemini-nano-banana-2-1-api-pt](https://github.com/n4bmw2s63acl/gemini-nano-banana-2-1-api-pt) · Русский [gemini-nano-banana-2-1-api-ru](https://github.com/ymtk2jpo85ct/gemini-nano-banana-2-1-api-ru)
- **More models in this language** — [midjourney-api-ja](https://github.com/apimartt1q/midjourney-api-ja) · [gemini-2-5-flash-image-preview-api-ja](https://github.com/ic7skkh/gemini-2-5-flash-image-preview-api-ja) · [nano-banana-api-ja](https://github.com/e4ylndl3g-v/nano-banana-api-ja) · [gemini-3-1-flash-image-preview-api-ja](https://github.com/gv4ju/gemini-3-1-flash-image-preview-api-ja) · [nano-banana-2-api-ja](https://github.com/lpbxkrzbfel/nano-banana-2-api-ja) · [gemini-3-pro-image-preview-api-ja](https://github.com/zzsxs8r/gemini-3-pro-image-preview-api-ja)
- **Topic hubs** — every build of this model, all languages: [gemini-nano-banana-2-1](https://github.com/topics/gemini-nano-banana-2-1) · [gemini-nano-banana-2-1-api](https://github.com/topics/gemini-nano-banana-2-1-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-818e99> | `apimart.ai/ja/model/nano-banana-2-1` |
| Live pricing | <https://go.apimart.ai/k-8e44ce> | `apimart.ai/ja/pricing` |
| Get an API key | <https://go.apimart.ai/k-3bc92b> | `apimart.ai/ja/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Gemini Nano Banana 2.1 is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
