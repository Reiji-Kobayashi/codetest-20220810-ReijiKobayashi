#　株式会社ゆめみ Android エンジニアコードチェック課題 開発記録 電気通信大学 小林怜史

## 開発環境
- macOS: 11.6(Big Sur)
- IDE: Android Studio Chipmunk | 2021.2.1 Patch 1
- Kotlin: 1.7.10
-
- gradle: 7.0.2
- minSdk 23
- targetSDK: 31

## 開発履歴
### issue1
- https://kotlinlang.org/docs/coding-conventions.html を参考にしつつ、目視で確認して修正。

### issue2-1
- エラー文：`Use requireContext() instead of context!!`
- エラー文に従い`context!!`を`requireContext()`に変更

### issue2-2
- 参考URL
  - https://www.project-unknown.jp/entry/2017/09/15/083241
  - https://runebook.dev/ja/docs/kotlin/docs/reference/typecasts

### issue2-3
- 該当変数`lastSearchDate`は即座に再代入が行われず、null許容型にしない必要もないため、null許容型に変更

### issue2-4
Strの`"null"`の握りつぶしが行われている部分を修正
該当変数を`val`から`var`に変更し、if文で修正できるようにした。

### issue2-5
commitメッセージに記述していないが、`"null"`の握りつぶしがあった部分も追加で修正

### issue3-1
- Warning文
  - `Do not concatenate text displayed with 'setText'. Use resource string with placeholders.`
  - `String literal in 'setText' can not be translated. Use Android resources instead.`
- Android Studio側の指示に従い、`@SuppressLint("SetTextI18n")`の挿入で解決
- 事前の文字列を格納する変数に代入して、その変数を用いる方法も考えられるが、~~コードが長くなりそうなので却下~~
  - SDKのバージョンに依存しているWarning文を表示しないようにしているだけで、実質的な解決になっていない

### issue3-2
- Warning文：`Hardcoded string "", should use (@string() resource`
- UIパーツにそのままテキスト入力をせず、`res/values/string.xml`に文字列を用意し、それをUIパーツ側で利用するようにする。

### issue3-3
- `@SuppressLint`SDKのバージョンに依存しているWarning文を表示しないようにしているだけで、実質的な解決になっていない
- 事前の文字列を格納する変数に代入して、その変数を用いる方法を採用

### issue3-4
- 参考URL
  - https://qiita.com/uny/items/1c1cf8e308bc68ea9f92
  - https://stackoverflow.com/questions/47628646/how-should-i-get-resourcesr-string-in-viewmodel-in-android-mvvm-and-databindi
- Warning文：`This field leaks a context object`
- 色々試した中、`AndroidViewModel`を使用した方法が一番早く解決した
- 今回の`context`の使用は`string.xml`の参照のみなので問題なさそうだが、今後の開発を考慮しても有効かどうかは不明

### issue3-5
- issue3-5だと`OneFragment.kt`のインスタンス宣言で問題が生じる
- 以下の理由で新たに関数を使用した文字列を作成を行なった
  - contextを使用している処理が1つだけであり、他の手法でのメモリリーク解決のコストが高いため
  - 作成する文字列が短く、平易であるため

### issue4-1
- 2つのクラスが混在したファイルから1つのクラスを新規ファイルに移動

## 備忘録
### githubへのpush取り消し（履歴）
- コードチェックと無関係の個人ファイルを誤ってcommitしたため使用
- issue2を最初からやり直すため使用

### GitHubへのcommitの取り消し（手法）
参考URL
- https://techtechmedia.com/cancel-remote-commit-git/
- https://qiita.com/S42100254h/items/db435c98c2fc9d4a68c2
1. GitHubにて戻し先のcommitIDを調べる
2. `git reset --hard commitID`でローカルリポジトリのcommitの取り消し
3. `git push -f origin commitID:branch_name`でGitHubリポジトリのcommitの取り消し
4. `git push origin branch_name`で戻し先から新たにcommit
