# CodinGame Hypersonic の戦略とか

2016/09/27 のんさこ

## 勝利条件

一番多くの箱を壊すと勝ち

## 探索方法

- DFS: 深いのでダメ
- BFS: そこまで広くないからいいかも
- ビームサーチ: 評価関数がきちんとできたら
- ちょくだい: う、うん、100msで何回できるか考えてみるとね、うん。
             でもメモリの量は問われてないからワンチャン

## 評価頻度など

1ラウンドごとに？1アクションごとに？相手の状態は読む？

->読みやすいので1ラウンドごとにする

## 評価関数

### 箱爆発予約

爆弾を置くことにより箱の爆発を予約する

- 爆発に要するターン数
- 爆発数

### アイテム取得

アイテムの取得によって動きやすく

- 種類ごとのアイテムの取得数

ただしこれは爆発後にしかアイテムの詳細がわからないためランダムに

プレイヤーのステータスの増加と考えればいい？

箱のあった場所に2/3の確率で半々でアイテム

-> 特殊なタイプのアイテムをスポーンさせよう！！

### 爆発数 (実装済)

実際に爆発した箱の個数

実際に点になるため高めに

### 爆発時間

速い方がいい

味方や相手の爆弾の連鎖を狙う

ただこれは評価が難 + 後々の計算によって必要なくなる

### 箱、アイテムまでの距離

何もないとこで途方に暮れるよりマシ

端っこチマチマプレイが少なくなるかも

### (後々追加) 巻き込まれによる減点

Ω＼ζ°)ﾁｰﾝ

## 評価用ステートオブジェクト: 内容

- その時点での箱や爆弾の位置
  - 後に出てくる壁について考慮して、箱や壁は2bitで1マスを表現？
  - **stringでもつのやめよう！！！！**
  - 爆弾やアイテムはエンティティのリスト？ (100個以下)
  - 箱とか壁もエンティティとして持っておいたほうが得？
- プレイヤーの情報
  - 位置
  - 爆弾の個数/残り個数
  - 爆弾のレンジ
- 壊した箱の個数

## ターン毎の行動

上下左右移動+**不動**

- 爆弾が置ける場合
  - 設置するかしないか 5*2通り
- 置けない場合
  - 移動するだけ 5通り