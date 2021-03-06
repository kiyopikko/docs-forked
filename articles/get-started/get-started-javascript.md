# JavaScript SDK

JavaScript(Web) で Milkcocoa を使う方法です。ライブラリのインストールが必要です。

なお、 JavaScript で Milkcocoa を使う場合、認証にアクセストークンを利用します。

## Milkcocoa のインストール

プロジェクトに [milkcocoa](https://www.npmjs.com/package/milkcocoa) をインストールします。

```bash
$ npm install milkcocoa
```

CDNの場合は以下です。

```html
<script src="https://cdn.mlkcca.com/v0.6.0/milkcocoa.js"></script>
```

## Milkcocoa に接続する

ブラウザで Milkcocoa を利用する場合はアクセストークンが必要です。最も簡単な方法は、以下のように Milkcocoa ユーザーの認証情報を使う方法です。

```js
// for browserify, webpack etc...
// var Milkcocoa = require('milkcocoa');

Milkcocoa.authWithMilkcocoa({appId: ${appid}}, function(err, milkcocoa) {
  // Use `milkcocoa` for any methods
});
```

実行をすると、ポップアップで Window が開いて Milkcocoa のアカウントで認証します。認証が完了したら、第二引数のコールバック関数が呼ばれ、 `milkcocoa` オブジェクトを利用できます。

なお、認証した Milkcocoa アカウントが、管理者もしくは Collaborator（管理画面のAccess > Collaborator）の場合のみ、API の利用が許可されます。

## データを Push（配信・保存）する

`DataStore` オブジェクトを利用して、データの Push を行います。

```js
milkcocoa.dataStore('demo/js', {datatype: 'json'}).push({'v':2});
```

`datatype` オプションでデータの型を指定できます。上記の例では JSON にしています。

## データを Subscribe（購読）する

`on()` を利用することで、Subscribe ができます。

```js
let ds = milkcocoa.dataStore('demo/js', {datatype: 'json'});

ds.on('push', function (datum) {
  console.log('Pushed: '+datum.value.v);
});

ds.push({'v':2});
```

実行すると、以下のようなログが出力されます。

```bash
# Log
Pushed: 2
```

データの形式は以下のようになっています。

```json
{
  id: "1e725aca-5613-1342-ae46-cdfe7f0db0a7",
  timestamp: 1492680909730,
  value: { v: 2 }
}
```

## データを取得する

`history()` を利用して、データの取得ができます。

```js
let ds = milkcocoa.dataStore('demo/js', {datatype: 'json'});

ds.history({}, function(err, messages) {
  console.log(err, messages);
});
```

なお、第一引数にはオプションとして、データ数、昇順・降順、期間の指定ができます。

```js
// Default options
{
  limit: 100,
  order: 'desc', // or 'asc'
  ts: null // このタイムスタンプより前のデータを取得する
}
```