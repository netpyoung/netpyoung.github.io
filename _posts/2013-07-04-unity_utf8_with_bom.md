---

layout: post
title: 'Unity UTF8 with BOM'
tags: unity3d BOM

---


## BOM이란?
* https://en.wikipedia.org/wiki/Byte_order_mark
* BOM이란 byte order mark의 약자로, 파일 혹은 스트림의 바이트 순서(엔디안)을 나타내기 위해 사용되는 Unicode 문자.

## UTF-8의 BOM은?
* `EF BB BF`

## 문제는?
* UTF8자체가 엔디안 문제가 생기지 않음. 즉 UTF8에는 BOM이 필요없다는 것.
* Unity에서 기본으로 소스코드를 생성하면 UTF8 without BOM문서가 나옴.
* 근데, 뻐킹 유니티에서는 BOM이 없으면 코드를 이상하게 읽음. (한글 포함시)
tip) 꼭 한글 주석 달고 마침표로 끝을 알려주자.

## 해결책은?
* Unity에는 AssetPostprocessor란 클래스가 있고, OnPostprocessAllAssets란 메소드가 있는데, 이를 이용하면 파일 생성시 마다 BOM을 추가시켜줄 수 있다.

# inspired
* http://debuglog.tumblr.com/post/26550984743/utf-8
 - https://github.com/sharkattack51/Unity-EditorScript/blob/master/AssetPostprocessUTF8Encode.cs

## example source

SimpleUtf8BOM.boo

```boo
import UnityEngine

import System.IO

class SimpleUtf8BOM (AssetPostprocessor):

	static def OnPostprocessAllAssets(
		imported_assets        as (string),
		deleted_assets         as (string),
		moved_assets           as (string),
		moved_from_asset_paths as (string)) :

		for asset in imported_assets :
			checkAndWriteBOM(asset)


	static def checkAndWriteBOM(fpath as string) :
		# check: is source file?
		ext = Path.GetExtension(fpath)
		unless (['.cs', '.js', '.boo']).Contains(ext) :
			return;

		# check: is with bom_utf8?
		fbytes = File.ReadAllBytes(fpath)

		front_3bytes = fbytes[:3]
		bom_utf8     = array(byte, (0xEF, 0xBB, 0xBF))

		# write bom
		unless front_3bytes == bom_utf8 :
			File.WriteAllBytes(fpath, bom_utf8 + fbytes)
			Debug.Log("append bom complete : ${fpath}")
```
