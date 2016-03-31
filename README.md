# C×3 How-To-Work

この資料はC×3で開発していくうえでの心構え的なのを載せておきます。

## はじめに
こんにちは！けっつんです。
某社のwelcome見て感動したので、ちょっと真似して作ってみました。ここに開発の仕方とかをまとめておくので、目を通したうえでわからないとこ調べて潰しておいてくれるとめちゃめちゃ助かります。


## 開発の進め方について

### ■ 開発言語
jacascriptを使用します。学習コストが一番少なく、かつバックエンドもフロントエンドも開発しやすい言語、スケールしやすい言語だからです。何か他に使いたい言語があれば言うてください。ぜひやってほしいです笑

#### ◆ バックエンド
けっつんがgo言語をやりたいので、go言語でapiサーバをたてます。←
基本的にはapiサーバを構築するまでをバックエンドとしています。例外はあると思います。

#### ◆ フロントエンド
Node.jsを使って開発します。基本的にはAngular.jsを使用して開発していきますが、習得具合で新しい刺激が欲しいな、と思ったらCycle.jsを使用して開発していきましょう。ちなみに、Cycle.jsは英語の記事ばっかりです。


### ■ ソースコードの管理について

![](http://kykamath.github.io/images/github.png)

[GitHub](https://github.com/)を利用します。

また、セキュリティレベル向上のためアカウント作成後に[二段階認証](https://help.github.com/articles/about-two-factor-authentication/)を有効にしてください。


### ■ プロジェクトの進捗管理

Trelloを使用します。

#### ◆ Trelloカード登録時に設定する項目

##### タスク名（Title）
長すぎず、完結に。
どこの画面の機能なのか、どこに関する作業なのかを明確に書くことを最初に。
```
例1). 【全体】UIデザイン作成
例2). 【管理画面】店舗情報編集機能
```
##### コメント（Comment）
コメントを見た人が、そのタスクを処理するときに何をすればいいのかがイメージできるように書いてください。
とりわけ追加することが必須というわけではありません。わかりやすくするために書いて欲しいな〜くらいです。

##### 担当者（Assignee）
担当者が決まっている場合は指定すること。

### ■ 作業開始から終了まで

基本的にgithub-flowの流れに則った形で作業を進めていきます。

参考 : [Understanding the GitHub Flow](https://guides.github.com/introduction/flow/)

- Trelloに作業内容を登録
- 作業用ブランチ作成
- 実装作業
- GitHubへ作業用ブランチをpush
- プルリクエストの作成
- 実装者以外のレビュー
- （レビューで修正の指定がある場合は修正後、再度レビューを依頼）
- 本ブランチ（主にmaster）へのマージ

#### ◆ 利用するブランチ
初期開発完了（受託開発であれば納品まで。サービスであれば初期ローンチまで）はmasterブランチをmerge先のブランチとします。

新機能を開発する際にはgit-flowに則り「features/your-task」のような形で新しくmasterからブランチを作成して作業を開始してください。

1つのブランチで複数の機能実装に対応しても問題ありませんが、多く詰め過ぎるとmergeが面倒になるので極力控えてね。Conflict起きたら自分で解決してもらうからね←

![](http://joefleming.net/images/posts/git-flow-timeline.png)

#### ◆ ローカルでのmergeについて
作業用ブランチにmasterをmergeするタイミングなど、ローカルでmerge作業を行う際には必ず「--no-ff」を指定してください。

```shell
# masterブランチを自分の作業用ブランチにmergeする場合
git merge master --no-ff
```

#### ◆ プルリクエストの作成
作業用ブランチでの作業が完了したら、masterブランチに対してプルリクエストを作成してください。その上でSlack上のプロジェクト用チャネルで

```
@誰々 pull-request-url

レビューお願いします。
```

のような形でSlack上で声をかけてください。（かけなくてもみます笑）

- このプルリクエストは何なのか？
- どこからチェックし始めればいいのか？（例えば起点/中心となるクラス）
- 実装の背景となることで何か特筆すべきことがあれば記述
- 質問事項（実装方針に関することや、いつマージすべきか、など）

参考 : [Pull request templates make code review easier](https://quickleft.com/blog/pull-request-templates-make-code-review-easier/)

#### ◆ issueの完了
もしバグが見つかった場合などは、issueを登録してもらいます。そのバグを回収しきったときは、gitでcommit時のメッセージとして「fixed #000」と入れておいてください。

こうしておくことで、masterにmergeされた時点でissueが自動的にcloseされます。

自動的にissueをcloseしたくない場合には、「#000」とだけ書いておけばissueに関連づけられます。

![](https://raw.githubusercontent.com/iro-dori/welcome/master/images/Trello-log.png)


### ■ テストコードについて
とりあえずスルー。


### ■ コメントについて
クラス単位でのコメントは最小限に（そもそもクラス名を見てその役割が分からないのはおかしいので）とどめる形で構いません。

関数単位でのコメントは

- その関数が行っていること
- 引数の説明
- 戻り値の説明
- その他補足事項

を出来る限り記述するようにしてください（以下の例を参照:Androidアプリの場合）。

```java
#
# アプリのメイン画面に関するクラス
#
public static class MainActivity {
  #
  # Viewをセッティングする
  #
  # ただViewを設定する関数です。
  #
  # @TODO Toolbar等に変更加えるときの変更
  #
  # @param [int] position そのviewを構成する要素のposition
  # @param [ViewGroup] parent そのviewを内包している親のviewgroup
  #
  # @return [View] 作成されたView
  #
	public View setView() {
		# ...
	}
}
```
