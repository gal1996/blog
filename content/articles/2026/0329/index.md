+++
date = '2026-03-29T00:00:00+09:00'
title = 'AIに障害を起こさせない開発~harness engineering~'
categories = ['エンジニアリング']
tags = ['ハーネスエンジニアリング', 'AI', 'Harness Engineering']
+++

# AIにいかに障害を起こさせずに開発させるか

直近このtopicについて考えている人は多いだろうし自分もその一人で、
このtopicについて考えている人はだいたいharness engineeringという言葉にたどり着くのではないかと思う。
今回はharness engineeringについて自分で調べた限りの情報と考えを書いてみる。

```
このブログ記事は、すべて私の手による、私自身の言葉で書かれたものです。
こんなことを言わなければならないのは残念ですが、特にこのテーマを考えると、
あえて明記しておきたいと思います。
```

# harness engineeringとは

そもそもharness engineeringという言葉を最初に使ったのは、ghosttyを作ってるMitchell Hashimoto氏ぽい（[Mitchell HashimotoのAI活用ブログ記事](https://mitchellh.com/writing/my-ai-adoption-journey)）。
この中では、harness engineeringはエージェントに正しい結果を出させるために、エージェントが誤りを自動的に検知できる高速で高品質なツールを提供すること。と言っている。
直近はharness engineeringはもうちょっと広い捉えられ方をしてきているのかなと思っていて、保証のないAIのアウトプットに対してどのように人間が責任を持てるような構造を作るか、みたいな感じになってきているような気もする。

# AIをリードしている各社のharness engineering

AnthropicやOpenAIなどAIをリードしている企業のブログ記事を元にharness engineeringについて考察してみる。

## OpenAIのharness engineering

この[OpenAIのharness engineering記事](https://openai.com/ja-JP/index/harness-engineering/)がharness engineeringという言葉を世の中に広めたんじゃないかという気がする。

この記事では社内向けアプリケーションのベータ版のリリースをエンジニア数人で5か月で100万行近くのコードを書いた事例を紹介している。

この記事ではアプリケーションが正しく動作することを保証するためのharnessというイメージでharnessが紹介されているように感じる。

なので、AGENTS.mdのような強制力の強くないツールよりも、テストやlintといった強制力を持たせられる自然言語を介さないツールを主に提供することを書いているように思う。まさにアプリケーション開発におけるharnessという感じがする。世の中で言われているharnessのイメージはおそらくこっちだと思う。

## Anthropicのharness engineering

一方で、claude codeを作っているAnthropicの言うharness engineeringは自分が想像しているものとは少し違った。

この記事の執筆時点では[AnthropicのHarness Design記事](https://www.anthropic.com/engineering/harness-design-long-running-apps)が最新だが、
この記事も基本的にはアプリケーションを正しく動作させることを想定しているが、エントリポイントが異なる。
この記事は「claudeを作って」というような、よりファジーな入力からより精度の高いアプリケーションを作らせるためのharnessという印象を持った。

常にプロンプトというファジーな入力を求められるclaude codeを作っている会社の視点という感じがする。

ただ、Anthropicもclaude codeはclaude codeに作らせているという発信があったようにOpenAIのようなharnessの設計ももちろん行っているんだと思う。

## Martin Fowlerのブログ記事

数々の名著を出しているMartin Fowler氏もこちらの[Martin FowlerのHarness Engineering記事](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)でharness engineeringについて言及している。

この記事は先のOpenAIのharness engineeringの記事に対しての批評というかたちで書かれている。

この記事はより実務のアプリケーション開発の立場で意見が書かれていて、
世の中の殆どのシステムはすでに開発が進んでいる巨大なアプリケーションになっている。
このアプリケーションにどのようにharnessを適用するのか？という疑問が書かれている。

実際自分も色々記事を読んでいて思うことだが、ある程度抽象的なhowとしてのharnessは語られているが、
より具体のテスト戦略やlintの設定などを書いている記事は殆どないように感じる。

これはそもそもツールもAIに作らせているから具体が見えないから書かれていないのか、まだそこまで明確な方針がないのかはよくわかってない。けど、具体を語っている記事が見当たらないことは事実だと思う。

# まとめ

ほぼ感想になってしまったが、自分で色々調べてみて、harness engineeringの重要性は感じる。
AIのアウトプットに対して人間がどう責任を持つのか？ということに答えるために、
AIのアウトプットの品質を担保するためのharnessなのだろうなと思う。

一方で、その具体はあまり語られておらず、明確な方針というのはまだ存在していなさそう。
だとすると、今自分たちができることはこれまで人間が開発していたときから当たり前に言われてきたことを突き詰めて行くことなのかなと思う。

テストのカバレッジを上げる、テストケースの把握容易性を上げる、lintを整備する、運用を考えたメトリクスの整備と運用を揃える。
これまで工数を言い訳として、人間がすべて実装しようとするとアウトプットスピードが落ちる、と言っていたことをAIを使って工数の問題を乗り越えキッチリやっていくことが現時点では最も重要なのかなという気がする。

## 余談

Mitchell Hashimoto氏のブログの中で、AIエージェントの作業が終わったときの通知は行わないほうがいいと言っていて面白いなと思った。
今通知を受けてエージェントを複数動かさせることが主流になっている気がするが、Mitchell Hashimoto氏は通知でタスクを切り替えると人間のコンテキストスイッチのコストのほうが高くなるから、
タスクの切り替えは人間側のタスクの切り替えタイミングに合わせるべきだと言っている。
マルチタスクが本当にできる人なら通知ベースで切り替えっても良いかもしれないが、
自分はマジで苦手なので通知切ってみようと思った。

そして、人間が本当に集中して取り組むべきタスクをAIに遮られることなく取り組むようにしてみようと思う。
