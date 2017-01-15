---
layout: default
title: "履歴書"
---

# 語学水準

日本語 | 初級 (まだ、テストしていません（N4程度) 話は少しできます。書くこと読むことは下手です。)
英語   | 中級 (技術文書の読解。英語での基本的な会話が可能。)
{: .table .table-bordered}


# 履歴

.	              | .
------------------|----------------------
2002.03 ~ 2005.02 | 済州第一高等学校
2005.03 ~ 2005.07 | 済州大学(家庭の事情中途退学)
2006.11 ~ 2008.11 | 軍隊
2009.03 ~ 2010.11 | 職業学校
2011.08           | 學點銀行制 - 学士(電子計算学)
2012.04 ~ 2013.12 | Ncrew Entertainment
2014.01 ~ 2014.07 | Sonaclo
2014.08 ~ 2015.11 | GREE
2016.01 ~ 2016.03 | 渋谷外語学院
2016.07 ~ .       | 株式会社ファンクルー
{: .table .table-bordered}



# 私ができること(会社でやったこと)
# Unity client engineerで備えた能力
* メインプログラマーとしてプロトタイピングからの発売や、運営までまでゲームシステムの大部分を具現したことがあります。
* サーバーサイドも学びながら助けたことがあります。
* データ変換及び管理プロセスの構築。(excel, csv, json, sqlite, msgpack)
* ゲームマネージャー設計や具現(patch, build, UI(ngui, ugui, fairtygui), sound, log,  define, lua binding,tutorial, encrypt)
* Unity Editor.(C#define編集、スキル確認ツール, リアルタイムマスターデータロード, マスターデータクラス自動生成, キャラクター、モンスターウェイポイント編集, デザイナーデータ自動)
* Android/iOS native plugin.
 - https://github.com/netpyoung/unity.native-example
 - https://github.com/netpyoung/unity.libsodium
 - https://github.com/netpyoung/SqlCipher4Unity3D
* AI using BehaviorTree.


# その他
* DirectX 9 and Orge Engine, Scaleformを利用してゲームを開発してみた経験があります。
* clojureでゲームサーバーをclojurescriptにhtml5 canvasを利用してスロットマシン、カードゲームfacebookゲームを作ってみたことがあります。
* git/mercurial/svnを扱って見ました。 中でgitを好きです。
* chatting botとjenkinsを連動したことがあります
* pythonを利用してサーバ作業をしたことがあります。
* goを利用してサーバapiテストをしたことがあります。
* linux環境でも拒否感なく作業が可能です。
* nvimとemacsを使用可能です。 emacsをもっと好みます。
* 新たな環境や学問に拒否感がありません。


# なぜ日本?
* 幼い時、日本の文化(ゲーム(英雄伝説,竜機伝承, 幻世酔虎伝..)と漫画映画、ドラマ)に関心を持つようになりました。
* GREE一ヶ月日本出張でもっと関心を持つようになりました。
* 確信がなかったから、3ヵ月間語学校で勉強をし、シェアハウスで生活してみました。
* 様々な日本の友達と会ってみたいし、日本のゲーム制作文化についてもっと知りたくて日本に来るようになりました。


# よく聞かれる質問
* 頻繁な転職に対する心配
  - 日本とは違って韓国では、職員に対する発電より会社の利益について欲が深いです。 したがって、プロジェクトが終わる単位で転職が起こるのです。 韓国にいるときは普通だと思ったけど、日本では少し文化が違うようです。
* なぜ今会社から
  - 日本のゲーム開発の文化をもっと長く知りたいですので、日本の会社に入って一緒に仕事をしたいです。


# どんな人
* 新しい技術を勉強して共有していることから面白さを感じます。。
* ゲームロジックに集中したくて自動化をたくさんします。
* 同じチームであるにクライアントがプログラムで他のパート人たちの不便さを改善することにも関心が多いです。







## Detail
# lua binding

* テュートリアル、バトルをlua scriptで作成して企画者が実験してみたいとしている内容をすぐに確認して実験して見ることができます。

```
=> c# function <=> lua function binding
=> lua command => C# command queue
=> using lua coroutine.yield()
=> c# command process
=> sync or async(coroutine) process like tutorial, in-game
```

``` csharp
script.Globals ["Character"] =(Func<string, string, TARGET_POS, TalkCharacter>)TalkCharacter.GenCharacter;

string scriptCode =  File.ReadAllText (Application.dataPath + "/res_r/lua/quest.lua");
ScriptMachine machine1 = new QScriptMachine();
yield return machine1.Init();
StartCoroutine(machine1.Run(scriptCode));
```


``` csharp
public class QuestCommand_Talk : BaseQuestCommand
{
	TalkCharacter character;
	string message;
	object[] args;
	public QuestCommand_Talk(TalkCharacter character, string message, params object[] args)
	{
		this.character = character;
		this.message = message;
		this.args = args;
	}
	protected override IEnumerator Execute (QuestSequenceController controller)
	{
		yield return controller.Talk (character, message, args);
	}
}
```

``` lua
    local rumi = Character("rumi", "img_1", TARGET_POS.BOTTOM)
    rumi:Show()
    rumi:Talk("helloworld)
    rumi:Deactive()
    rumi:Hide()
    coroutine.yield()
    local answer = Question("like me", "yes", "no")
    if answer == "yes" then
        print("test")
    else
        print("test")
    end
    coroutine.yield()
```


# patch, build
* 毎回人の手にするとミスによるビルドエラーの集中力減少を書くときにない資源浪費。
* 企画者がビルドを言って作業物をすぐに確認できます。
* デザイナー・リソース・変換作業を自動で処理します。
* これで作業者はゲームロジックに集中できる時間が増えます。

```
[Planner PC]
 => [Slack] : planner typing command likes `!build JPN DEV`
 => [Slack bot] : parsing user command and run jenkins web api
 => [Jenkins] : build custom user script

* Run Build Script Written in Ruby Rakefile
 (windows batch、powershell、bash shell scriptいずれも扱うと思うが
これまでに経験でruby Rakefileが管理するよかったです。命令語一つにビルドおよび配布までしました。)

# client master data
 => [Google Sheet]
 => [Google Api] : download excel to build machine.
 => [Converting] : excel to sqlite.
 => [Custom api] : Upload sqlite to client data patch server.
 => [Git] : versioning excel and exported file.

# build
 => [Git] : pull client source.
 => [Unity ExecuteMethod] : auto versioning
 => [Android] : using mustache to generate AndroidManifest.xml.
 => [iOS] : modify `Info.plist`, `project.pbx` run xcodebuild commandline.
 => [Hockey Api] : Upload build file

* assetbundleの生成(aws-api利用してアップロード)とunittestも同じような過程で行われます。

```


[lostinstars.link]: https://youtu.be/zaUb5cVU1nU
[lostinstars.jpg]: {{ site.Paths.resources }}/pic/lostinstars.png
[devilmaker.link]: http://www.palmple.com/devilmaker/movie
[devilmaker.jpg]: {{ site.Paths.resources }}/pic/devilmaker.jpg
[carte.link]: https://www.youtube.com/watch?v=4NicQp58-kE
[carte.jpg]: {{ site.Paths.resources }}/pic/carte.jpg
