# Unity C# Style Guide

本文書は、MESONにおけるUnity C#開発におけるコードのスタイル基準を定めるものである。  
これは原則であり、既存プロジェクトに手を加える場合やその他**すでにルールが存在するプロジェクトの場合はそちらの規約（あるいは暗黙的にそうなっているルール）に従う**。



## 目的

1. 意図を明確にする
2. 一貫性を保つ
3. 可読性を高める



## Table of Contents

- [フォーマットルール](#フォーマットルール)
  - [空白](#空白)
    - [インデント](#インデント)
  - [改行](#改行)
- [クラス定義](#クラス定義)
- [メソッド定義](#メソッド定義)
- [ネーミングルール](#ネーミングルール)



## フォーマットルール

- ガード節が使えそうな場合は積極的に利用する。  
  ※ ただし、if文の中が1〜2行の場合はif文の中に処理を記述する。
  
  ```c#
  // ガード節が使えるケース
  private void AnyMethod()
  {
  	if (_anyField == null)
  	{
  		return;
  	}
  
  	// any process.
  }
  
  // 内容が1〜2行の場合
  private void AnyMethod()
  {
  	if (_anyField != null)
  	{
  		AnotherMethod();
  	}
  }
  ```
  
- `var` は原則使用しない。ただし、  `foreach` 内など見づらくなるケースや自明な場合は使用可。  
  (e.g. `var apple = new Apple();` , `var hoge = GetComponent<Hoge>();` )  
  　　また、関数に渡すためだけの一時的な変数。 (e.g. `var item = GetItem(); ProcessItem(item);` )
- コメントの // のあとは半角スペースを空ける（見やすさのため）。
- if文の中に多くの判定を入れない（多くても2つまで）それ以外は変数に入れて意味を記述する。

  ```c#
  if ((isApple || isOrange) && (isFoo || isBar)) { }
  
  // OK
  bool isFruits = isApple || isOrange;
  bool isEtable = (isFoo || isBar);
  if (isFruit && isEtable) { }
  ```

- 原則として `this` は指定しない。


### 空白

#### インデント

- インデントにはソフトタブを使用し、4文字スペースを採用する。


### 改行

- クラス定義、メソッド定義、if文など中括弧を用いる際は改行して中括弧から開始するようにする。

  ```c#
  if (hoge)
  {
      // do something.
  }
  ```

  

## クラス定義

- クラス名は大文字始まりにする。
- クラスのメンバには必ず `private` / `protected` / `public` など修飾子を付ける。
- 原則としてメンバは `private` とする。公開が必要と判断されたもののみ、 `public` をつけて公開すること。
- プライベートフィールドはアンダースコア始まりにする。（e.g. `_hogeField` ）
- メソッド名は `public` / `private` 関わらず大文字始まりにする。
- 失敗する可能性があるメソッドの場合は「 `Try` 」を先頭に付け、戻り値は `out`で返すようにする。
- 定数には `readonly` を付け、すべてを大文字にして単語を `_` で区切る。
- インスペクタの `OnClick` イベントなどにアタッチして利用することを想定しているメソッド名には、Prefixとして `Inspector` を付与する。（e.g. `InspectorHoge` ）
- `[SerializeField]` を利用する際は改行を入れず、続けて宣言を行う。（e.g. `[SerializeField] private float _hoge = 0;` ）
- フィールドには必ず初期値を代入する。
- `namespace` は必ず付与する。
- `using` はファイル先頭で宣言し、 `namespace` の中には含めない。



## メソッド定義

- Updateメソッド内には直接処理を記述しない。（該当処理を別のメソッドに切り出す）
- イベントハンドラと実際の処理は分けて記述する。（該当処理を直にアタッチしない。ハンドラを常に定義することで後の検索性を上げる）

  ```c#
  // NGパターン
  private void Initialize()
  {
  	_anyButton.onClick.AddListener(AddItem);
  }
  
  private void AddItem()
  {
  	// do adding process.
  }
  
  // ----------------------------------------------
  
  // OKパターン
  private void Initialize()
  {
  	_anyButton.onClick.AddListener(OnClicked);
  }
  
  private void OnClicked()
  {
  	AddItem();
  }
  ```

  

## プロパティ定義

- プロパティは可能であれば常に `=>` を使って定義を行う。



## ネーミングルール

- インターフェースは`I`始まりで定義する。（e.g. `IHogeable` ）
- ローカル変数名は省略せず意味の分かる名称にする。
    - ただし、インクリメント用など寿命が短く自明な場合は短い変数名も可。
- ローカル変数は `camelCase` で記述する。
