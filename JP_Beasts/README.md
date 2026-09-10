# JP Beasts — 和ノ獣譚 (RimWorld MOD)

RimWorld **1.6** 向けの XML-only MOD です。自然発生する敵生物(野生の危険生物)を **14種** 追加します。
「妖怪」系(人型)と「古代種」系(獣・虫型)の2系統。NPCファクション/キャラクターは追加しません。

全14種とも `trainability: Intermediate` に設定しており、**テイム(手懐け)が可能**です
(初期バージョンでは `trainability: None` にしていましたが、これはRimWorldでは「訓練不可」だけでなく
「テイム自体が不可能」を意味する値だったため修正しました)。テイムの難易度自体は `Wildness` や
`manhunterOnTameFailChance` の値で個体ごとに変わるため、アダマンタイトや竜人のような危険な個体は
テイムに失敗しやすく、失敗するとマンハンター化するリスクがあります。

## 追加される生物

### 妖怪タイプ (人型・`body: Human` を流用)

| 生物 | defName | 特殊能力 | combatPower |
|---|---|---|---|
| 竜人 | `JP_Dragonkin` | 火(爪にBurn追加ダメージ)、移動が速い | 210 |
| 鬼人 | `JP_Oni` | 強打(拳に気絶効果) | 150 |
| 天狗 | `JP_Tengu` | 超能力(※注1、ステータスのみ) | 110 |
| 妖狐 | `JP_Youko` | 火(爪にBurn追加ダメージ) | 90 |
| 雪女 | `JP_Yukionna` | 冷気(爪にFrostbite追加ダメージ) | 90 |
| 猫又 | `JP_Nekomata` | 移動が速い | 85 |
| 座敷童 | `JP_Zashikiwarashi` | テイムすると稀に銀を産出(※注2) | 20 |

### 古代種タイプ (獣・虫型・`QuadrupedAnimalWithPawsAndTail` を流用)

| 生物 | defName | 特殊能力 | combatPower |
|---|---|---|---|
| アダマンタイト | `JP_AdamantiteBeast` | 硬い、移動が遅い、超大型、卵生 | 280 |
| マンモス | `JP_Mammoth` | 移動が遅い、超大型 | 190 |
| マゾタイロス | `JP_Mazotairos` | 硬い(重装甲)(※注3) | 175 |
| ティタノプテラ | `JP_Titanoptera` | 攻撃的な待ち伏せ型、卵生(※注3) | 130 |
| エゾオオカミ | `JP_EzoWolf` | 移動が速い、大型 | 100 |
| ニホンオオカミ | `JP_JapaneseWolf` | 移動が速い、大型 | 80 |
| メガネウラ | `JP_Meganeura` | 飛行(高速)、小型・脆弱、卵生(※注3) | 25 |

(※注1) 天狗の「超能力」: 能動的に発動できるアビリティとしての実装は、Royalty/Anomalyのアビリティシステムか
専用のC#コードが必要なため未実装です。高い`PsychicSensitivity`ステータスと描写のみの対応です。

(※注2) 座敷童の「稀に銀を生産する」: `CompProperties_Milkable`を銀の産出に転用した実験的な実装です。
繁殖システム(`CompProperties_EggLayer`)とは独立したコンポーネントのため、通常交配に影響しません。
テイムして懐かせた個体からのみ、長い間隔(20日)で少量(8個)の銀が採れます。

(※注3) マゾタイロス・メガネウラ・ティタノプテラは詳細指定がなかったため、「古代種」というテーマに沿って
Claude側で推測・補完したオリジナル設定です(マゾタイロス=重装甲の草食巨獣、メガネウラ=水辺の巨大古代トンボ、
ティタノプテラ=地を這う攻撃的な巨大古代昆虫)。

## 毛・革・肉の入手方法(修正済み)

ご指摘いただいた通り、以下の仕組みで統一しています:

- **毛 (Wool)**: `CompProperties_Shearable` により、**テイムして懐かせた個体を定期的に毛刈り**することで入手。
  生きた個体からのみ得られ、死体からは得られません。
- **革 (Leather)**: 種族の `leatherDef` として設定。**屠殺 (butcher)** した死体から入手。通常の革と同様、
  あらゆる革製レシピにそのまま使用可能。
- **肉 (Meat)**: 種族の `meatDef` として設定。**屠殺**した死体から入手。

毛・革は種族ごとではなく、以下の系統で共有されます:

| 系統 | 対象生物 | 毛 (Wool) | 革 (Leather) |
|---|---|---|---|
| 古代種 | マンモス/ニホンオオカミ/エゾオオカミ/マゾタイロス | `JP_Wool_AncientBeast`(古代種の剛毛) | `JP_Leather_AncientBeast`(古代種の厚革) |
| 稀種 | 猫又/妖狐/雪女/座敷童/天狗/鬼人/竜人 | `JP_Wool_RareSpecies`(稀種の輝毛) | `JP_Leather_RareSpecies`(稀種の輝革) |
| アダマンタイト | アダマンタイト | `JP_Wool_Adamantite`(スカイスチール) | `JP_Leather_Adamantite`(アダマンタイトの硬革) |
| 古代虫 | メガネウラ/ティタノプテラ(毛なし、革のみ) | - | `JP_Chitin_Ancient`(古代虫の甲殻) |

肉は生物ごとに個別です。妖怪タイプ7種は全てバニラの `Meat_Human`(人肉)を共有します。古代種タイプは
`JP_Meat_Mammoth` / `JP_Meat_JapaneseWolf` / `JP_Meat_EzoWolf` / `JP_Meat_Adamantite` / `JP_Meat_Meganeura` /
`JP_Meat_Mazotairos` / `JP_Meat_Titanoptera` をそれぞれ専用に持ちます。

**加工(なめし)レシピは廃止しました** — 前バージョンにあった「生毛皮→革に加工」というクラフト工程は
ユーザー様のご指摘通り誤った設計だったため削除し、上記の直接入手方式に統一しています。

## 妊娠条件(繁殖)

- **妖怪タイプ7種**: `通常の交配`(`hasGenders: true` によるバニラの通常繁殖)で子供が生まれます。
  「異性の人間キャラクターに世話をされた場合に高確率で妊娠する」というボーナス条件はXML単体では実装
  できないため、**通常の同種交配のみ**が機能します。
- **アダマンタイト/メガネウラ/ティタノプテラ**: `卵生`。`CompProperties_EggLayer` + `CompProperties_Hatcher`
  による本物の孵化システムです。受精卵(`JP_Egg_*Fert`)は一定日数で孵化、未受精卵(`JP_Egg_*Unfert`)は
  食用・売却用アイテムになります。
- **その他の古代種タイプ**(マンモス/ニホンオオカミ/エゾオオカミ/マゾタイロス): `通常の交配`。

## 火・冷気・強打などの特殊能力について

RimWorldのXMLで安全に実装できる範囲で対応しています:

- **火 (妖狐・竜人)**: 爪の攻撃に `extraMeleeDamages` で `Burn`(火傷)ダメージを追加。
- **冷気 (雪女)**: 爪の攻撃に `extraMeleeDamages` で `Frostbite`(凍傷)ダメージを追加。
- **強打 (鬼人)**: 拳の初撃(`surpriseAttack`)に `Stun`(気絶)ダメージを追加。
- **移動が速い/遅い、大型/超大型、硬い**: `MoveSpeed` / `baseBodySize` / `ArmorRating_*` ステータスの調整。
- **超能力 (天狗)**: 上記の注1のとおり、能動的なアビリティとしては未実装です。

## 自然出現バイオーム

`Patches/Patch_BiomeWildSpawn.xml` で既存バイオームの `animalCommonalities` に追加しています。詳細な
組み合わせはファイルを参照してください。テスト時は開発者モードの「Spawn pawn」機能で即座に呼び出せます
(検索欄に `JP_` と入力すると全14種が絞り込めます)。

## MOD互換性 (Alpha Animals との併用について)

[Alpha Animals](https://steamcommunity.com/sharedfiles/filedetails/?id=1541721856)(作者: Sarg Bjornson、
packageId `sarg.alphaanimals`、1.5/1.6対応、Harmony + Vanilla Expanded Framework 依存)との併用を想定して
以下の点を確認・対応しています。

- **defName衝突なし**: JP_Beastsは全defNameに `JP_` を、Alpha Animalsは `AA_` をプレフィックスしているため、
  種族・素材・レシピいずれも名前が衝突しません。
- **継承元Abstractが共通**: 両MODとも同じバニラの `AnimalThingBase` / `WoolBase` / `LeatherBase` /
  `OrganicProductBase` / `EggFertBase` / `EggUnfertBase` を継承しています(実際、本MODの1.6対応フィールド名
  ―`Wildness`のStat化、`MeatBase`が存在しない件、`CompProperties_Shearable`/`Milkable`/`EggLayer`の
  正確なフィールド名―は、稼働実績のあるAlpha Animalsの実装を参照して検証しました)。そのため技術的な
  衝突リスクはありません。
- **バイオーム出現パッチは加算方式**: 両MODとも `PatchOperationAdd` で `animalCommonalities` に追記する
  だけなので、ロード順に関係なく安全に積み重なります。`About.xml` に `sarg.alphaanimals` への
  `loadAfter` ヒントを追加していますが、Alpha Animalsをインストールしていなくても問題なく動作します
  (未インストールなら単に無視されます)。
- **素材の相互運用性**: `JP_Wool_*` / `JP_Leather_*` / `JP_Chitin_Ancient` は通常のStuffアイテムなので、
  Alpha Animals側が追加する防具・衣服レシピでもそのまま素材として選択できます(逆にAlpha Animals側の
  革/毛皮もJP_Beastsに関係するレシピで使えます)。
- **バランス面の注意**: Alpha Animalsは非常に多くの動物(約100種)を低頻度で追加するため、併用すると
  マップ上の野生動物の絶対数・密度が上がります。JP_Beastsのレア個体(アダマンタイト・竜人など)の希少感を
  より際立たせたい場合は、`Patches/Patch_BiomeWildSpawn.xml` の該当 `<commonality>` 値をさらに下げる
  ことをおすすめします(現状の値のままでも動作上の問題はありません)。

## ファイル構成

```
JP_Beasts/
├── About/
│   ├── About.xml
│   └── Preview.png
├── Defs/
│   ├── ThingDefs_Races/
│   │   ├── Races_Ancient.xml            … 古代種7種
│   │   └── Races_Yokai.xml              … 妖怪7種
│   ├── ThingDefs_Items/
│   │   ├── Materials.xml                … 毛(Wool)・革(Leather)・甲殻(Chitin)
│   │   ├── Meats.xml                    … 専用の肉7種
│   │   └── Eggs.xml                     … 卵生3種の受精卵/未受精卵
│   └── PawnKindDefs/PawnKinds_Monsters.xml … 14種の個体設定
├── Patches/
│   └── Patch_BiomeWildSpawn.xml         … 既存バイオームへの野生出現テーブル追加
└── Textures/                             … プレースホルダー画像
```

## インストール方法

1. `JP_Beasts` フォルダごと RimWorld の `Mods` フォルダにコピーします。
2. Mod一覧から `JP Beasts - 和ノ獣譚` を有効化してゲームを再起動。
3. 開発者モードでログにエラーが出ていないか確認してください。

## プレースホルダー画像について

`Textures/` 以下の画像はすべて Pillow で自動生成した簡易シルエットです。差し替える場合は同じファイル名・
フォルダ構成のまま上書きしてください。

## 既知の制約

- Royalty/Ideology/Biotech/Anomaly 等のDLC固有バイオームには出現パッチを当てていません。
- 天狗の超能力・座敷童の銀産出は上記の通り近似実装/実験的実装です。
- 「異性の人間による世話での妊娠ボーナス」はC#アセンブリが必要なため未実装です。
- マゾタイロス・メガネウラ・ティタノプテラは詳細未指定のため推測で設定を補完しています。
