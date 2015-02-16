#CookieGetterSharp
ブラウザのCookieを.NETアプリで使えるようにするライブラリです。 <http://com.nicovideo.jp/community/co235502> で配布されているCookieGetterSharpを元に、見つけた変な部分などを修正したものです。.NET3.0以上で動きます(動作未確認)。.NET4.5.1で動作確認をしています。

## ライセンス
以下のライセンス下にあります。  
* CookieGetterSharp  
  https://github.com/namoshika/CookieGetterSharp  
  Copyright (c) 2014 namoshika.  
  Released under the **GNU Lesser GPL**  

以下の著作物から派生させています。
* [CookieGetterSharp](http://d.hatena.ne.jp/halxxxx/20091212/1260649353)  
  Copyright (c) 2014 halxxxx, うつろ

##方針
現在、以下のブランチの方針下にあります。  
現在はここから更にフォークし、[SnkLib.App.CookieGetter](https://github.com/namoshika/SnkLib.App.CookieGetter) の方で好きにやっています。
* base: 本家そのまま
* master: 見つけた不具合の修正。  
  (SnkLib.App.CookieGetterとの方針の違いは保守的で触らぬ神に祟りなしである事)

##各クラスの役割
以下の設計になっています。  
利用者が使用する際には主にCookieGetterの静的メソッドからICookieGetterを取得して使う形になります。

* static CookieGetter: 内部に保持するIBrowserManager群を管理し、ICookieGetterの生成を行う。
* instance IBrowserManager: ブラウザ毎にCookieStatusとそれを保持するICookieGetter生成。  
  (CookieGetter内部でICookieGetterの生成を行うファクトリクラスとして使われています)
* instance ICookieGetter: 内部に保持するCookieStatusの情報を使ってCookieの取得を行う。  
* instance CookieStatus: 各ブラウザのCookieファイルの場所などを保持。  
  (対象のICookieGetterが使用可能であるかやUI周りの機能などの怪しいものがあります)

##使い方
```C#
using Hal.CookieGetterSharp;

//なるべく独自にブラウザリストを生成せず、
//下のようにCreateInstancesメソッドから動的にリストを生成してください。
var importableBrowsers = CookieGetter.CreateInstances(true);
comboBox1.Items.AddRange(importableBrowsers);

//Cookieの取得は以下のようにします。
importableBrowsers[0].GetCookie(new Uri("http://live.nicovideo.jp/"), "user_session")
//あるいはこんな感じ。
importableBrowsers[0].GetCookieCollection(new Uri("http://live.nicovideo.jp/"))

//返り値を接続の都度、CookieContainerにセットしてください。
//Hal.CookieGetterSharp.BrowserTypeを設定の保存復元に使わないでください。
```
