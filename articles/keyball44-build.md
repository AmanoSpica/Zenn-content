---
title: "ついにKeyball44をつくる"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["keyball", "keyball44", "自作キーボード", "contest2024"]
published: true
published_at: "2024-12-24 17:00"
---

前から気になっていたKeyballをつくります。

## そもそもKeyballって？
Keyballとは[@Yowkees](https://github.com/Yowkees)様によって開発されたトラックボール付き左右分割キーボードです。自作キーボードの部類に入ります。

![](/images/keyball44-build/IMG_1518.jpg)
https://github.com/Yowkees/keyball

#### 最高なところ
- 親指でトラックボールを操作 → マウスまで腕を動かさなくていい
- 分割キーボード → 肩にやさしい
- 自作キーボード → キースイッチ・キーキャップが自由
- かっこいい。唯一無二。大好き。

### なぜKeyball44?
KeyballにはKeyball39/44/61の3種類が存在します。
39だと、英字以外のキーが親指キーしかないためハードルが高すぎると感じ、61だと普通のキーボードすぎかと思って44にしました。(これが最高でした)

## 部品集め
ちょうど東京に行く機会があったので秋葉原の遊舎工房さんにお邪魔して選んできました。

最終的に以下の部品を揃えました。
| 品名                                                                                                                                                                     | 数量 | 価格 (購入時) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---- | ------------- |
| [Keyball44](https://shop.yushakobo.jp/products/8337?_pos=3&_sid=b35737a9e&_ss=r)                                                                                         | 1    | 24,800        |
| [Pro Micro Type-C版](https://shop.yushakobo.jp/products/3905?pr_prod_strat=e5_desc&pr_rec_id=ada36a522&pr_rec_pid=7752927543527&pr_ref_pid=6055704756385&pr_seq=uniform) | 2    | 2,640         |
| [トラックボール 白 (amazon)](https://www.amazon.co.jp/dp/B0D4F4HMXD?ref=ppx_yo2ov_dt_b_fed_asin_title)                                                                   | 1    | 1,999         |
| [Kailh Midnight Silent V2 Switch / Linear](https://shop.yushakobo.jp/products/4270)                                                                                      | 45   | 2,840         |
| [Kailhロープロファイルスイッチ - Red Pro](https://shop.yushakobo.jp/products/pg1350?variant=44079245426919)                                                              | 5    | 330           |
| [MBK Choc Low-Profile Keycaps - White](https://shop.yushakobo.jp/products/mbk-choc-low-profile-keycaps?variant=37706914791585)                                           | 5    | 825           |
| [DSA 無刻印キーキャップ 1U](https://shop.yushakobo.jp/products/dsa-blank-keycaps?variant=37665598734497)                                                                 | 50   | 2,200         |
| [SK6812MINI-E（10個入り）](https://shop.yushakobo.jp/products/sk6812mini-e-10)                                                                                           | 6    | 2,310         |
| [TRRSメタルケーブル 0.8m](https://shop.yushakobo.jp/products/8111?_pos=1&_sid=509d55d53&_ss=r)                                                                           | 1    | 1,100         |
|                                                                                                                                                                          |      |
| 合計                                                                                                                                                                     |      | 39,044        |

![](/images/keyball44-build/IMG_1468.jpg)

### その他買ったもの
| 品名                                                                                                 | 価格  | メモ                                                                                        |
| ---------------------------------------------------------------------------------------------------- | ----- | ------------------------------------------------------------------------------------------- |
| [TypeC マグネットアダプタ](https://www.amazon.co.jp/dp/B0C53JSMLG?ref=ppx_yo2ov_dt_b_fed_asin_title) | 1,638 | USBに負担をかけないため。付け外しも楽になって最高です。                                     |
| [サンワダイレクト ポーチ](https://www.amazon.co.jp/dp/B0843WY8RR?ref=ppx_yo2ov_dt_b_fed_asin_title)  | 1,660 | ケース。Keyball44にはMサイズがジャストフィットです。                                        |
| テンティングセット (メルカリ)                                                                        | 9,882 | 詳しくは後述します。本来なら11,000円ぐらいするところをメルカリで9,000円台で入手できました。 |

### キースイッチの選択基準
遊舎工房さんにはキースイッチテスターがあり、販売されているほぼすべてのキースイッチを試して購入できます。今回は以下の基準でキーキャップを選びました。

- [Kailh Midnight Silent V2 Switch / Linear](https://shop.yushakobo.jp/products/4270)
  - 打ち心地が気持ちいい
  - ほどよい重さ (軽すぎず重すぎず)
  - 静かめ
  - 軸がぶれない
- [Kailhロープロファイルスイッチ - Red Pro](https://shop.yushakobo.jp/products/pg1350?variant=44079245426919)
  - 親指用なのでとにかく軽いものを
  - でも押している感じはするもの

## 作業開始
keyballは自作キーボードですのではんだ付け作業・組立作業が必要です。公式の[ビルドガイド](https://github.com/Yowkees/keyball/blob/main/keyball44/doc/rev1/buildguide_jp.md)に沿って作業を進めます。
はんだごては祖父にもらったものを使用しました。(工業系大学卒)

### 作業風景
![](/images/keyball44-build/IMG_1469.jpg)
![](/images/keyball44-build/IMG_1473.jpg)

LED 点灯テスト
![](/images/keyball44-build/IMG_1508.jpg)
![](/images/keyball44-build/IMG_1509.jpg)

完成間近
![](/images/keyball44-build/IMG_1510.jpg)

完成
![](/images/keyball44-build/IMG_1518.jpg)

普段からはんだ付けをしているわけではないのでめちゃめちゃ疲れました...。作業時間はだいたい4時間ぐらいだと思います。

LEDはつけなくてもよかったのですが、どこかの記事で「つけた方が完成の時、気分が上がる」と言われたのでつけてみました。
いいですね、これ。全部ついた時の達成感はヤバいです。

## ファームウェア書き込み
KeyballはQMKファームウェアという自作キーボードで汎用的に利用されているファームウェアを使用します。作業はすべてGoogle Chromeで行えます。

↓ 書き込もうとしている様子
![](/images/keyball44-build/remap-keys_flash-firmware.png)

もちろん、ファームウェアも自作することができます。沼です。
次の記事で書こうかな...。

## キーマップ
Keyballは完全に自由なキーマップを設定できます。沼です。
こちらもGoogle Chrome上で[Remap](https://remap-keys.app/)というサイトを使用して設定できます。

↓ 現在のキーマップ。最適解ではない。
![](/images/keyball44-build/key-map.png)

## テンティング
Keyballはテンティングをすることで本領を発揮します。
テンティングとは、キーボードをななめに置いてテントのようにすることです(？)。

詳しくはこちらの動画をご参照ください。(6:59あたり)
https://youtu.be/QC791UY_M6Y?si=ftF67OKr8wgkladn

動画で紹介されているものとまったく同じものを購入しました。
あるのとないのとではまったく違います。テンティングはKeyballユーザー全員がやるべきです。
(だいぶ高いのがデメリット。スマホスタンドでも実現している人もいるので試してみてください。)

## 使用した感想
- 明らかに疲れが少ない。自然な姿勢でタイピングできるので本当に疲れないです。
- キー配置が沼。まったく決まりません。
- キー配置が覚えられない。記号は鬼畜です。
- 完璧なキー配置を完璧に覚えるとすごい速いタイピングになる**気がする。**
- トラボがついてるのでいちいち腕を動かさなくていい。これが一番でかいです。
- 真ん中が空いているから書類とか置ける。iPadおいてます。
- 圧倒的満足感

## まとめ
Keyballは最高。遊舎工房さんの実店舗で触れるので一度体感してみては。
