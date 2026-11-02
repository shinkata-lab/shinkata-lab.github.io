# ひまクイ！今日の3問（GitHub Pages 移設版）

- 本体 `index.html` は、ChatGPT（どんちゃん）制作の単一HTML v2.2 を移設したもの。
- 移設時にふわが行った変更は次の2点だけ（サイト設置・運営側の調整）：
  1. 挑戦状に付くサイトURL（SHARE_SITE_URL）を移設先 `https://shinkata-lab.github.io` に差し替え
  2. 未使用だった SUZURI_PRODUCT_URL 定数を除去（v2.2で販売枠は削除済みだったが定数だけ残っていた）
- GPTから新しい版が来たら、この2点のパッチを当て直してから差し替えること。
- `about.html` / `privacy.html` は AdSense 審査・公開運営用の文章ページ（ふわ作成）。

## v2.3（2026-11-02 差し替え）
- GA4計測を同意バナー方式で組み込み（測定ID: G-BJL73RRLW3）。同意前はgtagを読み込まない。独自イベントは daily_view / quiz_start / quiz_complete の3種のみ（同一ブラウザ・同日1回）。同意キーはゲーム記録と分離（himaqui.analytics.v1.consent）。
- privacy.html をGA4対応の改定版に更新。トップの注意書きも実態に合わせて差し替え済み。
- 問題データ・出題スケジュールは v2.2 から無変更（ふわが文字列単位で照合済み）。移設パッチ2点（シェアURL・SUZURI定数除去）は v2.3 納品時点で適用済みだったため再適用なし。

## v2.3.1 CSP修正（2026-11-02 ふわ施工）
- 症状：GA4タグは発火するがヒットが1件も送信されない。Tag Assistantで原因特定：CSPの img-src が googletagmanager.com への画像ビーコン（/a?id= ヒット）を拒否していた（"Refused to load the image ... img-src 'self' data: blob:"）。
- 修正：img-src に https://www.googletagmanager.com と https://www.google-analytics.com を追加（この1行のみ）。GPTに次版でCSPへ反映するよう別途連絡すること。
