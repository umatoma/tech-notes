SOLID原則
-----

ソフトウェア設計をより理解しやすく、柔軟で、メンテナンスしやすくする為の５つの原則である。

`SOLID`とは以下の５つの原則の頭文字を取ったものである。
- Single Responsibility Principle | 単一責任の原則
- Open Closed Principle | オープン・クローズドの原則
- Liskov Substitution Principle | リスコフの置換原則
- Dependency Inversion Principle | 依存関係逆転の法則
- Interface Segregation Principle | インターフェース分離の原則


# Single Responsibility Principle | 単一責任の原則
クラスを変更する理由は１つ以上存在してはならない。

## 概要
１つのクラスは１つの役割（＝変更理由）だけ持つべきである。  
複数の役割を持っていると、何かを変更した際にそのクラスが持っている他の役割にも影響を与えてしまう可能性があるので良くない。

## 悪い例
Rectangleクラスが数学的な値の計算と図形の描画という２つの役割を持ってしまっている。

![SRP_BAD](images/single_responsibility_principle_bad.png)

## 改善例
![SRP_GOOD](images/single_responsibility_principle_good.png)


# Open Closed Principle | オープン・クローズドの原則


# Liskov Substitution Principle | リスコフの置換原則


# Dependency Inversion Principle | 依存関係逆転の法則


# Interface Segregation Principle | インターフェース分離の


# 参考文献
- http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod
- https://en.wikipedia.org/wiki/SOLID
- アジャイルソフトウェア開発の奥義 第2版
