Clean Architecture - 読書メモ 1
-----

# 設計とアーキテクチャ

ソフトウェアアーキテクチャ

- 目的
  - 求められるシステムを構築・保守するために必要な人材を最小限に抑えること
  
崩壊したコードを書く方がクリーンなコードを書くより常に遅い

- TDDありとTDDなしで比較
  - TDDを使ったほうが速い
  
速く進む唯一の方法はうまく進むこと

- 優れたソフトウェアアーキテクチャを設計するということ
- クリーンアーキテクチャ


# ２つの価値のお話

ソフトウェアシステムの価値

- 振る舞い
- 構造 >> こちらの方が重要
  - 簡単に変更できるようにする
    - ソフトであるということ
  - 「形状」ではなく「スコープ」に比例して難易度が変わるべき
  

# アーキテクチャとは？

アーキテクチャ

- 主な目的
  - システムのライフサイクルをサポートすること
    - 開発
      - 開発しやすくなるようなシステムにする
    - デプロイ
      - 単一のアクションで簡単にデプロイ出来るようにする
      - 速い檀家来からデプロイ戦略を考える
    - 保守
      - 保守派最もコストがかかる
      - アーキテクチャを考え抜きコストを下げる
      
ソフトウェアの価値

- 振る舞いの価値
- 構造の価値
  - ソフトウェアをソフトにする価値
    - できるだけ長い期間・多くの選択肢を残すこと
    - 残すべき選択肢
      - 重要ではない詳細
        - DB・サーバー・フレームワーク・プロトコル...


# 独立性

優れたアーキテクチャがサポートすること

- システムのユースケース
- システムの運用
- システムの開発
- システムのデプロイ

レイヤーの切り離し

- UI・ビジネスルール・データベース等のレイヤーで切り離す
- 変更される理由をまとめる

ユースケースの切り離し

- ユースケースで切り離す
- 他のユースケースに影響なく、新しいユースケースを追加する

レイヤー・ユースケースを切り離すことで

- 「独立した開発」「独立デプロイ」を可能とする

切り離し方式

- ソースレベル
  - モジュールを分離する
  - モジュール間で影響しない
- デプロイレベル
  - デプロイ可能な単位を分離する
  - ソースコードの変更が他のモジュールに影響しない
- サービスレベル 
  - 実行単位を完全に分離する
  - ソースやバイナリの変更が影響しない


# バウンダリー：境界線を引く

ソフトウェアアーキテクチャとは、境界線を引く技芸である

- バウンダリー
  - 「重要なもの」と「重要でないもの」の間に引く
  - 重要なもの
    - ビジネスルール
  - 重要でないもの
    - DB・GUI...
    
TODO: 図を入れる


# 方針とレベル

コンピュータプログラムは、入力を出力に変換する「方針」を記述したもの

- レベル
  - 入力と出力からの距離
- 依存方向はレベルと結びつける


TODO: 図を入れる


# ビジネスルール

エンティティ

- ビジネスデータを操作するビジネスルールを含んだオブジェクト
  - ビジネスルール
    - システムが自動化されていなくても存在するルール
  - ユースケースのことは知らない

ユースケース

- 自動化されたシステムを使用する方法を記述したもの
  - ユーザーインターフェイスについては記述しない
  - エンティティのことを知っている


# 書籍
