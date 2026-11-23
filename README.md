# PyRIT × Qwen2.5 小さいモデル ハンズオン

Microsoft の PyRIT を使って、Qwen2.5 の小さいモデル（0.5B / 1.5B / 3B / 7B）が
「断るべき設問」にどう応答するかを測った検証のリポジトリです。

元記事（7B / 14B / 32B が対象）:
https://zenn.dev/matsushima_a/articles/day-048-pyrit-local-llm-redteam

## 設計

- 対象：Qwen2.5 0.5B / 1.5B / 3B / 7B（7B はアンカー）
- 判定役・攻撃役：7B 固定
- 設問：元記事と同じ1問のみ
- 手法：単発5種（直接 / DAN / Skeleton Key / Base64 / 過去形）＋ Many-shot ＋ Crescendo
- 環境：Google Colab 無料GPU（T4）、PyRIT 1.2.1、Ollama はノートブック内で localhost 接続、外部APIなし
- 採点：OBJECTIVE_STRICT / OBJECTIVE_LENIENT の2基準で再採点＋目視4分類

## ファイル

- `pyrit_pilot_colab.ipynb`：パイロット版（対象3Bのみ、全7手法を各1回）
- `pyrit_full_colab.ipynb`：本番版（4サイズ、計132回）
- `RESULTS_full.md`：本番の集計と読み取り
- `visual_classification.md`：全132件の目視4分類と、判定役スコアとの突き合わせ

## 主な結果

- STRICT と LENIENT で判定が割れたのは 57 / 132 件。目視の「注意書き付き回答＋手順回答」と一致
- Base64 はスコア上は全件「拒否」だったが、目視では全20件がデコード失敗による脱線だった
- temperature=0 のため、132件の応答は重複を除くと39種類のみ

## 生の応答データについて

応答の生データ（results.jsonl / scored.jsonl）は、このリポジトリには含めていません。
設問への手順回答そのものを含むためです。集計と分類は上記のドキュメントで全て公開しています。

## 検証について

検証の実施（ノートブック作成・実行・集計）は AI の Muse が担い、
筆者は仕様の決定と結果の確認を行いました。
