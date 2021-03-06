詳細仕様書
=========
## 変更履歴
- 2014.01.02 初版
- 2014.01.11 修正

## 機能一覧
機能一覧は以下の通りとする。
- ツイート
- 単語登録
- 単語更新
- 単語検索
- 単語削除

## 機能詳細
### ツイート
- (作成中)

### 単語登録
#### URI
/word

#### HTTPリクエスト
##### リクエストメソッド
POST

##### リクエストヘッダ
```
 Content-Type: application/json;charset=UTF-8
```

##### リクエストボディ
###### フォーマット 
```
{
  “word” : “単語”,
  “description” : “単語の意味”,
  “example” : “単語の例文”
}
```

###### リクエスト   
```
{
  “word” : “わい”,
  “description” : “私、僕、俺。”,
  “example” : “わいがモテないのはどう考えてもおめどが悪い。”,
  “translation” : “私がモテないのはどう考えてもお前らが悪い。”
}
```

#### HTTPレスポンス
##### HTTPステータスコード
| ステータスコード | 説明             |
|:----|:-----------|
| 201 | 正常終了 |
| 400 | HTTPリクエストエラー |
| 500 | サーバ内部エラー |

##### レスポンスヘッダ
```
 Content-Type: application/json;charset=UTF-8
```

##### レスポンスボディ
###### 正常終了
```
 { “id” : “単語ID”, "word" : "単語" }
```

例   
```
 { “id” : 2, "word" : "わい" }
```


###### 異常終了
[エラー情報]の[フォーマット]を参照。

下表に、エラー別のエラー情報を記載する。

| エラー内容 | エラーコード | エラーメッセージ | エラー詳細 |
|:----------:|:-----------:|:------------:|:------------:|
| リクエストボディが空 | 11000001 | empty_body | なし |
| 取得した値が空 | 11000003 | empty_value | エラーメッセージ |
| 入力値文字数超過 | 11000003 | value_exceeded | エラーメッセージ |

例 : リクエストボディが空  
```
“error” : { “code” : “11000001”, “message” : “empty_body”, “detail” : “” }
```

例：入力値文字数超過(wordカラムが20文字)
```
“error” : { “code” : “11000003”, “message” : “value_exceeded”, “detail” : “Word is too long” }
```

### 単語更新
- (作成中)


### 単語検索
#### URI
/word?q="単語"&match_type="(complete | part)"

q

- 単語。指定なしの場合は全件検索。

match_type  

- complete : 完全一致。指定なしでも可。
- part : 部分一致。部分一致の場合は指定必須。

#### HTTPリクエスト
##### リクエストメソッド
GET

##### リクエストヘッダ

```
 Content-Type: application/x-www-form-urlencoded
```


##### リクエストボディ
なし

#### HTTPレスポンス
##### HTTPステータスコード
| ステータスコード | 説明             |
|:----|:-----------|
| 200 | 正常終了 |
| 400 | HTTPリクエストエラー |
| 404 | 単語が見つからない |
| 500 | サーバ内部エラー |

##### レスポンスヘッダ
```
 Content-Type: application/json;charset=UTF-8
```

##### レスポンスボディ
###### 正常終了
JSON配列形式で返す。   
```
[
  {
    "id" : 単語ID,
    “word” : “単語”,
    “description” : “単語の意味”,
    “example” : “例文”,
    “translation” : “例文の訳文”,
    "created_at" : 登録日時
  },...
]
```

例   
```
[
  {
    "id" : 2,
    “word” : “わい”,
    “description” : “私、僕、俺。”,
    “example” : “わいがモテないのはどう考えてもおめどが悪い。”,
    “translation” : “私がモテないのはどう考えてもお前らが悪い。”,
    "created_at" : 2014-01-01T10:10:10Z'
  }
]
```

###### 異常終了
[エラー情報]の[フォーマット]を参照。

単語が見つからない場合は、404エラーとし、エラー情報は返さない。  
上記以外については、下表に、エラー別のエラー情報を記載する。

| エラー内容 | エラーコード | エラーメッセージ | エラー詳細 |
|:----------:|:-----------:|:------------:|:------------:|
| 検索条件が間違っている | 11000003 | match_type_incorrect | 間違った検索条件 |

例 : 検索条件が間違っている(存在しない検索条件:aaa)  
```
“error” : { “code” : “11000003”, “message” : “match_type_incorrect”, “detail” : “aaa” }
```

### 単語削除
- (作成中)

## データベース定義

### テーブル定義
使用するテーブルは以下の通りとする。
- 単語テーブル (Words)

#### 単語テーブル
テーブル定義は下表の通りとする。

| カラム | 型 | 制約 | 説明 |
|:----------:|:-----------:|:------------:|:------------:|
| id | integer | primary key | 単語ID。自動インクリメント。 |
| word     | varchar(16) | unique key | 単語。 |
| description | varchar(32) | not null | 単語の意味。 |
| example | varchar(64) |  | 単語の例文。 |
| tranlation | varchar(64) |  | 例文の訳文。 |

### シーケンス定義
使用するシーケンスは以下の通りとする。
- 単語IDシーケンス (Word_Id_Seq)

#### 単語IDシーケンス
このシーケンスは、Wordsテーブルにレコードを登録するときに、id値を+1する。


## エラー情報
### フォーマット

```
“error” : {
  “code” : “エラーコード”,
  “message” : “エラーメッセージ”,
  “detail” : “エラー詳細”
}
```