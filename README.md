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
- 修正：img-src に https://www.googletagmanager.com と https://www.google-analytics.com を追加（この1行のみ）。

## v2.4 解析ローダー標準化（2026-11-02、GPT制作・ふわ検証）
- v2.3.1でもヒット0件が継続。切り分けの結果：GAプロパティ正常（直接送信テスト着弾）／素のgtagテストページは本番CSP付きでも計測成功／よって原因はv2.3のカスタムローダー（dataLayer再代入・二重定義・リトライ・ロック・ハードストップ）と特定。
- v2.4で解析まわりだけを標準スニペット方式に置換（dataLayer初期化1回のみ、同意後に consent default → js → config、独自イベントは単純なlocalStorage台帳＋標準gtag('event')）。問題データ・文言・デザイン・CSPは不変（CSPはv2.3.1の文字列のままGPT側にも反映済み）。
- privacy.htmlは撤回時の文言を「次回の読み込みから計測を停止」に合わせて1行だけ改訂。
- 診断用テストページ ga-test.html / ga-test-csp.html は検証完了後に削除すること。

## v2.5 コラム追加（2026-11-03、GPT制作・ふわ検証）
- コラム3本を独立ページで追加：column-nakamahazure.html（仲間外れの読み方）／column-zukei.html（図形の数え方）／column-suuretsu.html（数列の規則）。例題は自明なオリジナル例のみで、出題中・今後の問題と答えは含まない（ふわが全文を読んで確認済み）。
- index.html：ホームに「読み物（コラム）」導線、フッターに about/privacy リンク、結果画面に「今日の3問の振り返り」（QUESTIONS原文の正解・解説を再掲）を追加。問題データ・解析まわり・CSPは不変（diff照合済み）。
- 同期対象は7ファイルに増加（index/about/privacy/README＋コラム3本）。

## v2.5.1 AdSense審査準備（2026-11-03、ふわ施工）
- 運営者がAdSenseに申込（サイト shinkata-lab.github.io 登録、publisher ID ca-pub-2078950867616586）。サイト側の準備としてふわが実施：
  - 全6ページ（index/about/privacy/コラム3本）の <head> にAdSenseコードスニペットを設置。
  - CSPがある4ページ（index＋コラム3本）は script-src / img-src / connect-src に https://pagead2.googlesyndication.com を追加（about/privacyにはCSPなし）。
  - ads.txt をルートに追加（google.com, pub-2078950867616586, DIRECT, f08c47fec0942fa0）。
  - privacy.html の「広告について」を AdSense 導入に合わせた開示文に更新。
- 承認後にやること：広告枠の実装（結果画面の振り返りの下＋コラム内、手動配置）と、そのためのCSP追加（広告配信ドメイン）。自動広告の扱いは承認後に判断。
- 同期対象は8ファイルに増加（＋ads.txt）。

## v2.6 品質アップ＋AdMax設置（2026-11-03、GPT制作・ふわ検証/施工）
- GPT制作（画像生成含む）：マスコット（星型の黄色いキャラ＋水色スカーフ、透過WebP）— ホーム用1点＋結果画面の正解数別3点（perfect/happy/encourage、結果テンプレートに resultMascotHtml で出し分け）。コラム3本に固有イラスト（assets/column-*.webp）。OGタグを全ページに追加（og:image は assets/og-himaqui.png、原画はGPT生成・タイトル文字はふわがPILで合成）。
- ふわ検証：index.html の差分は4箇所のみ（CSS追加・OGタグ・ヒーローのマスコット挿入・結果マスコット出し分け）で、問題原文・進行ロジック・GA4・AdSenseスニペット・CSP基本構成は無変更（diff照合済み）。about/ads.txt は v2.5.1 と同一、privacy は下記の開示更新のみふわが変更。suzuri言及なし。
- 画像ファイル（assets/ 配下8点）はバイナリのためスナップショットpush対象外。GitHub Web画面からのアップロードで別コミットとして配置した。
- AdMax設置（ふわ施工）：忍者AdMaxの枠「ひまクイ 結果画面」(ID 1237882, SP 320×100, adm.shinobi.jp/s/a28f11c16071232fdc55944ec7a1c4ad) と「ひまクイ コラム」(ID 1237883, SP 320×100, adm.shinobi.jp/s/4965549f598c1394903f7fcdec146a5c) を作成（2026-11-03、作成直後はAdMax側で「審査中」表示）。
  - 結果画面：AdMaxタグは document.write 方式のため、結果テンプレート内の広告位置プレースホルダに iframe（ad-result.html、タグ単体の最小ページ、noindex）を差し込む方式。index の CSP は frame-src 'none'→'self' のみ変更。
  - コラム：各ページ末尾（</article>直前）にタグを直接設置。CSP は script-src に adm.shinobi.jp、frame-src に adm.shinobi.jp を追加し、img-src は https: に拡張（広告クリエイティブの配信ドメインが可変のため）。
  - privacy.html の「広告について」に忍者AdMaxを追記。
- 同期対象は9ファイル（＋ad-result.html）。assets/ は上記の通り別管理。
- v2.7 品質アップ第2ラウンド（ChatGPT提案①②③＋Gemini素材、2026-11-03）：ホームのマスコットをGemini生成のひらめきポーズに差し替え、「今日の3問、ひらめく準備は？」と開始ボタン「今日の3問に挑戦」を追加。結果画面のメッセージをスコア別の相棒リアクション文に変更し、満点マスコットはGemini生成のジャンプ絵＋一度だけ弾む演出（モーション軽減設定では無効）。コラム導線はサムネイル付きカード3枚に変更。問題データ・計測・広告まわりは無変更（問題データは文字列照合で完全一致を確認）。
  - 画像の配信方式：GitHubへの画像ファイル直接アップロードが本人確認の都合で滞ったため、当面は画像を data URI として各HTMLに埋め込む方式で公開する（サイト表示に必要な画像は全て埋め込み済み）。このため og:image は一時的に外している。画像ファイルを assets/ に配置できた時点で、通常のファイル参照方式に戻す。
- v3.0 デザイン刷新（2026-11-03、GPTデザイン画・ふわ施工）：全ページをフラットデザインに刷新。配色は深い紫#5B2D8E／黄色#FFC93C／クリーム#FFF8E7／インク#2A2140の4色に限定し、グラデーション・影を廃してベタ塗り＋2px線＋角丸に統一。画像ファイルを全廃し、ヒーロー・キャラクター・アイコン・コラム図版はすべてインラインSVG（コード描画）に置換（data URI埋め込みも解消、index は約285KB→約79KBに軽量化）。ホームの主見出しは実テキスト（h1）の2段構成「ひらめき、／3つ。」とし、星キャラが数字に触れる接点を追加。結果画面の星キャラ3表情は同系統に統一。ヘッダーの「読みもの」リンクはホーム表示時のみ表示。問題データ・進行・GA4・同意・AdSense・AdMax・CSPの基本構成は不変（問題データは文字列照合で完全一致を確認）。og:image は画像ファイルの配置経路が開くまで引き続き外したまま。GPT完成レビュー：OGを除くデザイン・静的レビューで95/100点（2026-11-03）。
