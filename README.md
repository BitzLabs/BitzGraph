# BitzGraph

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

BitzGraphは、BitzLabsエコシステム内でネットワーク図、フローチャート、UMLクラス図のような、静的な構造を持つダイアグラムを扱うための専門ライブラリです。

## 主な機能

-   **`GraphAST`の定義**: ノード(Node)、エッジ(Edge)、サブグラフ(Subgraph)、属性(Attribute)で構成される、汎用的なグラフ構造を表現するAST。
-   **2つの入力ルート**:
    1.  **パーサー**: DOT言語や、PlantUMLのクラス図などの構文を解釈し、`GraphAST`を生成します。
    2.  **専用API (Builder API)**: C#やTypeScriptのコード上で動的にノードやエッジを追加し、`GraphAST`をプログラム的に構築できます。
-   **レイアウトエンジン**: `GraphAST`を受け取り、グラフの自動レイアウトアルゴリズムを用いて各ノードとエッジの座標を計算し、その結果を描画プリミティブの集合である**`LayoutAST`**として出力します。（初期実装では外部のGraphvizエンジンとの連携を想定）

## ✅ 初期開発ToDoリスト

1.  **`GraphAST`の型定義**:
    *   `IGraphNode`インターフェース(`IAstNode`継承)を定義。
    *   `GraphNode`, `Node`, `Edge`クラスの「殻」を作成。`Attributes`プロパティも定義。
2.  **専用API (ビルダー) の設計**:
    *   `GraphBuilder`クラスを作成。`.AddNode("id", "label")`, `.AddEdge("fromId", "toId")`のようなメソッドで直感的にグラフを構築できるAPIを設計。
3.  **パーサーの骨格**:
    *   `DotParser`クラスを作成。
    *   `Parse(string dotScript)`メソッドを実装し、`a -> b;`のような最も単純なDOT言語をパースして`GraphNode`を生成するロジックを実装。
4.  **レイアウトエンジンのインターフェース**:
    *   `GraphLayoutEngine`クラスを作成し、`Render(IGraphNode node)`メソッドを定義。
    *   **外部エンジン連携**: `GraphAST`をDOT言語文字列にシリアライズし、Graphvizの`dot`コマンドを呼び出してレイアウト情報を取得するプロセスのための、基本的なラッパーを実装。

## このライブラリの位置づけ

主に`BitzDoc`から利用され、Markdown文書内に様々な構造図を埋め込む機能を提供します。また、システムの依存関係やアーキテクチャを可視化するためのSDKとしても活用できます。

このライブラリの最終的な出力は`LayoutAST`です。これをSVGなどの具体的な画像形式に変換する処理は、`BitzRenderers`に委譲されます。

### 利用シナリオ

-   **DSL経由**: 設計ドキュメント内にシステムのアーキテクチャ図やデータベースのER図を描画する。
-   **API経由**: ソースコードを静的解析し、クラス間の継承関係を動的にグラフとして可視化するツールを構築する。

### 依存関係

-   `BitzAstCore`
-   `BitzParser`
-   `BitzLayout` (出力型として)
