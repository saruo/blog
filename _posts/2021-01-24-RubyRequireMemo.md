---
layout: post
title:  "Rubyのrequireについてきになるところを調べてみた。"
categories: ruby
---
<!--
チートシート
https://gist.github.com/mignonstyle/083c9e1651d7734f84c99b8cf49d57fa
-->

Rubyのrequireの挙動についてしっかり理解できていなかったので。

* require
  * https://docs.ruby-lang.org/ja/latest/method/Kernel/m/require.html
* require_relative
  * https://docs.ruby-lang.org/ja/latest/method/Kernel/m/require_relative.html
* load
  * https://docs.ruby-lang.org/ja/latest/method/Kernel/m/load.html

$LOAD_PATHには
* カレント(スクリプトを実行した親プロセスがいるディレクトリ)は入らない。
* スクリプトが置いてあるディレクトリ($PROGRAM_NAMEのディレクトリ)のパスも入らない。

カレントが勝手に入るのはセキュリティ的にかなりアレな感じがするのでよく考えたらそりゃそうだ。

自分のプロジェクト用のライブラリをrequireするために取れる手段。
* 実行時に親プロセスがいるディレクトリとrequireに記載するパスを合わせて、ファイルを特定できる形に。
  * 実行環境依存。あまり良くなさそう。
* requireにフルパスをかく
  * 配置依存。これもほとんどの場合よくない。
* $LOAD_PATHにフォルダを追加する。
* require_relativeを使う。
    * __FILE__からの相対パスになる。

普通に使う分には、ほぼ自前プロジェクトならrequire_relativeという理解で問題はなさそうな気がする。
プロジェクトをまたぐなら、$LOAD_PATHとかを当てにする方がいいのかな。

このドキュメントがわかりやすいように思った。
https://i.loveruby.net/ja/rhg/book/load.html
Ruby Hacking Guideというドキュメントの一部。

これは確かに。
> それと、ときどき勘違いする人がいるのだが、どこでロードしてもクラスがロー ドされる先が変わったりはしない。例えば次のようにmodule文の中でロードし ても何の意味もなく、ファイルのトップレベルにあるものは全てトップレベル に置かれる。
