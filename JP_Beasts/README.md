# JP Beasts — 和ノ獣譚 (RimWorld MOD)

RimWorld **1.6** 向けの XML-only MOD です。自然発生する敵生物(野生の危険生物)を **19種** 追加します。
「妖怪」系(人型)と「古代種」系(獣・虫型)の2系統。NPCファクション/キャラクターは追加しません。

全19種とも `trainability: Intermediate` に設定しており、**テイム(手懐け)が可能**です。テイムの難易度は
`Wildness` や `manhunterOnTameFailChance` の値で個体ごとに変わるため、アダマンタイトや竜人のような
危険な個体はテイムに失敗しやすく、失敗するとマンハンター化するリスクがあります。

## ★ 設定シート (`Patches/Patch_CreatureSettings.xml`) について

**出現頻度・基本肉量・基本革量・基本毛量は、すべて `Patches/Patch_CreatureSettings.xml` 1ファイルに
集約されています。** バランス調整はこのファイルだけを編集すれば完結します(`Defs/` 側のファイルを
触る必要はありません)。

- ファイルの冒頭に日本語で使い方コメントがあります。
- モンスターごとに見出しコメントで区切られたブロックがあり、各ブロックの中に
  `<commonality>数値</commonality>`(出現頻度) / `<MeatAmount>数値</MeatAmount>`(基本肉量) /
  `<LeatherAmount>数値</LeatherAmount>`(基本革量) / `<woolAmount>数値</woolAmount>`(基本毛量、
  昆虫種は毛がないため項目なし)が並んでいます。数値を書き換えて保存するだけで、次回起動時から反映されます。
- **特定のモンスターの自然出現を完全にOFFにしたい場合**は、そのモンスターの「出現頻度」ブロック
  全体(見出しコメントから次の見出しコメントの手前まで)を削除するか、XMLコメントで囲んでください。
  それだけで自然スポーンしなくなります(テイム・繁殖は引き続き可能です)。
- 値はあくまで「他MODも含めた相対的な重み」なので上限はありません。Alpha Animals等の大型動物MODと
  併用していて出現が薄いと感じる場合は、遠慮なく大きな値にしてください。

この方式はRimWorldのXMLパッチ機能(`PatchOperationAdd`/`PatchOperationReplace`)を使った、いわゆる
「設定シート」パターンです。C#製のMOD設定画面(ゲーム内でスライダーを動かす方式)ではありませんが、
1つのテキストファイルを開いて数値を書き換えるだけで済むという点では同じ目的を達成しています。
(ゲーム内リアルタイム設定画面はC#アセンブリが必須のため、XML-onlyという本MODの方針上は実装していません)

## 追加される生物 (19種)

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

### 古代種タイプ - 獣 (`QuadrupedAnimalWithPawsAndTail` を流用)

| 生物 | defName | 特殊能力 | combatPower |
|---|---|---|---|
| アダマンタイト | `JP_AdamantiteBeast` | 硬い、移動が遅い、超大型、卵生 | 280 |
| マンモス | `JP_Mammoth` | 移動が遅い、超大型 | 190 |
| マゾタイロス | `JP_Mazotairos` | 硬い(重装甲)(※注3) | 175 |
| エゾオオカミ | `JP_EzoWolf` | 移動が速い、大型 | 100 |
| ニホンオオカミ | `JP_JapaneseWolf` | 移動が速い、大型 | 80 |

### 古代種タイプ - 虫 (chitin系、毛なし。全て卵生)

| 生物 | defName | 特殊能力 | combatPower |
|---|---|---|---|
| プルモノスコルピウス | `JP_Pulmonoscorpius` | 毒針(Stab攻撃にToxicBite追加ダメージ)(※注3) | 140 |
| メガラクネ | `JP_Megarachne` | 森に潜む待ち伏せ型捕食者(※注3) | 110 |
| アースロプレウラ | `JP_Arthropleura` | 硬い(重装甲)、鈍重(※注3) | 90 |
| ティタノプテラ | `JP_Titanoptera` | 攻撃的な待ち伏せ型(※注3) | 130 |
| メガネウラ | `JP_Meganeura` | 飛行(高速)、小型・脆弱(※注3) | 25 |
| タイタノミルマ | `JP_Titanomyrma` | 群れ(herdAnimal)で行動(※注3) | 55 |
| アーキミラクリス | `JP_Archimylacris` | 高速で逃走、繁殖力が非常に強い(※注3) | 20 |

(※注1) 天狗の「超能力」: 能動的に発動できるアビリティとしての実装は、Royalty/Anomalyのアビリティシステムか
専用のC#コードが必要なため未実装です。高い`PsychicSensitivity`ステータスと描写のみの対応です。

(※注2) 座敷童の「稀に銀を生産する」: `CompProperties_Milkable`を銀の産出に転用した実験的な実装です。
繁殖システム(`CompProperties_EggLayer`)とは独立したコンポーネントのため、通常交配に影響しません。
テイムして懐かせた個体からのみ、長い間隔(20日)で少量(8個)の銀が採れます。

(※注3) マゾタイロス・メガネウラ・ティタノプテラ・プルモノスコルピウス・メガラクネ・タイタノミルマ・
アーキミラクリスは詳細指定がなかったため、「古代種」というテーマに沿ってClaude側で推測・補完した
オリジナル/実在古生物モチーフの設定です(マゾタイロス=重装甲の草食巨獣、メガネウラ=水辺の巨大古代トンボ、
ティタノプテラ=地を這う攻撃的な巨大古代昆虫、プルモノスコルピウス=実在の巨大サソリ属がモチーフ、
メガラクネ=実在の巨大な蜘蛛型古生物がモチーフ、タイタノミルマ=実在の巨大アリ属がモチーフ、
アーキミラクリス=実在の巨大ゴキブリ属がモチーフ)。

## 毛・革・肉の入手方法

- **毛 (Wool)**: `CompProperties_Shearable` により、**テイムして懐かせた個体を定期的に毛刈り**することで入手。
  生きた個体からのみ得られ、死体からは得られません(昆虫種は毛なし)。
- **革 (Leather)**: 種族の `leatherDef` として設定。**屠殺 (butcher)** した死体から入手。通常の革と同様、
  あらゆる革製レシピにそのまま使用可能。
- **肉 (Meat)**: 種族の `meatDef` として設定。**屠殺**した死体から入手。

毛・革は種族ごとではなく、以下の系統で共有されます:

| 系統 | 対象生物 | 毛 (Wool) | 革 (Leather) |
|---|---|---|---|
| 古代種(獣) | マンモス/ニホンオオカミ/エゾオオカミ/マゾタイロス | `JP_Wool_AncientBeast`(古代種の剛毛) | `JP_Leather_AncientBeast`(古代種の厚革) |
| 稀種 | 猫又/妖狐/雪女/座敷童/天狗/鬼人/竜人 | `JP_Wool_RareSpecies`(稀種の輝毛) | `JP_Leather_RareSpecies`(稀種の輝革) |
| アダマンタイト | アダマンタイト | `JP_Wool_Adamantite`(スカイスチール) | `JP_Leather_Adamantite`(アダマンタイトの硬革) |
| 古代虫 | メガネウラ/ティタノプテラ/プルモノスコルピウス/メガラクネ/アースロプレウラ/タイタノミルマ/アーキミラクリス(毛なし) | - | `JP_Chitin_Ancient`(古代虫の甲殻) |

肉は生物ごとに個別です(全12種、専用のMeat ThingDefを持ちます)。妖怪タイプ7種は全てバニラの
`Meat_Human`(人肉)を共有します。

各生物の実際の肉量・革量・毛量は **設定シート(`Patches/Patch_CreatureSettings.xml`)で一括管理**しています
(前述)。`MeatAmount`/`LeatherAmount` は「体格1.0あたりの基礎量」で、実際の産出量は個体の体格
(`baseBodySize`、`Defs/ThingDefs_Races/` 内で定義)を掛けた値になります。

## 妊娠条件(繁殖)

- **妖怪タイプ7種 + 獣タイプ5種(マンモス/ニホンオオカミ/エゾオオカミ/マゾタイロス)**: `通常の交配`
  (`hasGenders: true` によるバニラの通常繁殖)で子供が生まれます。「異性の人間キャラクターに世話をされた
  場合に高確率で妊娠する」というボーナス条件はXML単体では実装できないため、**通常の同種交配のみ**が
  機能します。
- **虫タイプ7種(アダマンタイトを含む) + アダマンタイト**: `卵生`。`CompProperties_EggLayer` +
  `CompProperties_Hatcher` による本物の孵化システムです。受精卵(`JP_Egg_*Fert`)は一定日数で孵化、
  未受精卵(`JP_Egg_*Unfert`)は食用・売却用アイテムになります。

## 火・冷気・毒・強打などの特殊能力について

RimWorldのXMLで安全に実装できる範囲で対応しています:

- **火 (妖狐・竜人)**: 爪の攻撃に `extraMeleeDamages` で `Burn`(火傷)ダメージを追加。
- **冷気 (雪女)**: 爪の攻撃に `extraMeleeDamages` で `Frostbite`(凍傷)ダメージを追加。
- **毒 (プルモノスコルピウス)**: 毒針の攻撃に `extraMeleeDamages` で `ToxicBite`(毒)ダメージを追加。
- **強打 (鬼人)**: 拳の初撃(`surpriseAttack`)に `Stun`(気絶)ダメージを追加。
- **移動が速い/遅い、大型/超大型、硬い**: `MoveSpeed` / `baseBodySize` / `ArmorRating_*` ステータスの調整。
- **超能力 (天狗)**: 能動的なアビリティとしては未実装です(前述の注1)。

## 自然出現バイオーム・出現頻度

`Patches/Patch_CreatureSettings.xml` に集約されています(前述の「設定シート」を参照)。
テスト時は開発者モードの「Spawn pawn」機能で即座に呼び出せます(検索欄に `JP_` と入力すると全19種が
絞り込めます)。

## MOD互換性 (Alpha Animals との併用について)

[Alpha Animals](https://steamcommunity.com/sharedfiles/filedetails/?id=1541721856)(作者: Sarg Bjornson、
packageId `sarg.alphaanimals`、1.5/1.6対応、Harmony + Vanilla Expanded Framework 依存)との併用を想定して
以下の点を確認・対応しています。

- **defName衝突なし**: JP_Beastsは全defNameに `JP_` を、Alpha Animalsは `AA_` をプレフィックスしているため、
  種族・素材・レシピいずれも名前が衝突しません。
- **継承元Abstractが共通**: 両MODとも同じバニラの `AnimalThingBase` / `WoolBase` / `LeatherBase` /
  `OrganicProductBase` / `EggFertBase` / `EggUnfertBase` を継承しています(本MODの1.6対応フィールド名
  ―`Wildness`のStat化、`MeatBase`が存在しない件、`MeatAmount`/`LeatherAmount`がstatBases上のstatである件、
  `CompProperties_Shearable`/`Milkable`/`EggLayer`の正確なフィールド名―は、稼働実績のあるAlpha Animalsの
  実装を参照して検証しました)。そのため技術的な衝突リスクはありません。
- **バイオーム出現パッチは加算方式**: 両MODとも `PatchOperationAdd` で `animalCommonalities` に追記する
  だけなので、ロード順に関係なく安全に積み重なります。`About.xml` に `sarg.alphaanimals` への
  `loadAfter` ヒントを追加していますが、Alpha Animalsをインストールしていなくても問題なく動作します
  (未インストールなら単に無視されます)。
- **素材の相互運用性**: `JP_Wool_*` / `JP_Leather_*` / `JP_Chitin_Ancient` は通常のStuffアイテムなので、
  Alpha Animals側が追加する防具・衣服レシピでもそのまま素材として選択できます(逆にAlpha Animals側の
  革/毛皮もJP_Beastsに関係するレシピで使えます)。
- **バランス面の注意(実プレイで確認済み)**: Alpha Animalsは非常に多くの動物(約100種)を同じバイオームの
  出現テーブルに追加するため、`animalCommonalities` は「全MOD合算した中での相対的な重み」で選ばれる関係上、
  素の値では出現がかなり埋もれてしまうことが実プレイで確認されました。設定シート側の値は単独導入時の
  目安よりだいぶ高めに設定済みです。それでも出にくいと感じる場合は、設定シートの該当値をさらに
  引き上げてください。

## トラブルシューティング(実プレイで確認済みの事例)

- **死体を解体できない/「解体できる死体がマップに存在しない」と表示される**
  解体台・解体スポットのビル詳細設定で、「Allowed corpses(許可する死骸)」のフィルターを確認してください。
  死体そのものに赤い禁止マークが付いている場合は、それを解除すれば解決します。
- **自然スポーンがなかなか起きない**
  RimWorldの野生動物補充は「日」単位でゆっくり進む仕組みのため、ゲーム内でまだ半日〜1日も経っていない
  段階では1体も出現しないのは珍しくありません。数日プレイしても全く出ない場合は、設定シートの
  `commonality` 値をさらに引き上げてください。

## ファイル構成

```
JP_Beasts/
├── About/
│   ├── About.xml
│   └── Preview.png
├── Defs/
│   ├── ThingDefs_Races/
│   │   ├── Races_Ancient.xml            … 古代種(獣)5種
│   │   ├── Races_Insects.xml            … 古代種(虫)5種(新規追加分)
│   │   └── Races_Yokai.xml              … 妖怪7種
│   ├── ThingDefs_Items/
│   │   ├── Materials.xml                … 毛(Wool)・革(Leather)・甲殻(Chitin)
│   │   ├── Meats.xml / Meats_Insects.xml       … 専用の肉(獣7種/虫5種)
│   │   └── Eggs.xml / Eggs_Insects.xml         … 卵生種の受精卵/未受精卵
│   └── PawnKindDefs/
│       ├── PawnKinds_Monsters.xml       … 14種の個体設定
│       └── PawnKinds_Insects.xml        … 新規昆虫5種の個体設定
├── Patches/
│   └── Patch_CreatureSettings.xml       … ★設定シート(出現頻度・肉量・革量・毛量を一括管理)
└── Textures/                             … プレースホルダー画像
```

## インストール方法

1. `JP_Beasts` フォルダごと RimWorld の `Mods` フォルダにコピーします。
2. Mod一覧から `JP Beasts - 和ノ獣譚` を有効化してゲームを再起動。
3. 開発者モードでログにエラーが出ていないか確認してください。

## プレースホルダー画像について

`Textures/` 以下の画像はすべて Pillow で自動生成した簡易シルエットです。差し替える場合は同じファイル名・
フォルダ構成のまま上書きしてください(画像の解像度はゲーム内の見た目の大きさに影響しません。大きさは
各種族の `drawSize` で制御されます)。今回追加した5種の昆虫(`JP_Arthropleura` / `JP_Pulmonoscorpius` /
`JP_Megarachne` / `JP_Titanomyrma` / `JP_Archimylacris`)もプレースホルダー画像のままなので、必要に
応じて差し替えてください。

## 既知の制約

- Royalty/Ideology/Biotech/Anomaly 等のDLC固有バイオームには出現パッチを当てていません。
- 天狗の超能力・座敷童の銀産出は上記の通り近似実装/実験的実装です。
- 「異性の人間による世話での妊娠ボーナス」はC#アセンブリが必要なため未実装です。
- マゾタイロス・メガネウラ・ティタノプテラ・プルモノスコルピウス・メガラクネ・タイタノミルマ・
  アーキミラクリスは詳細未指定のため推測で設定を補完しています。
- 設定シート(`Patch_CreatureSettings.xml`)はゲーム内リアルタイム設定画面ではなく、テキストファイルを
  編集して保存し、ゲームを再起動することで反映される方式です(Alpha Animals等のC#製MODにある
  「ゲーム内オプション画面」とは仕組みが異なりますが、1ファイルで一括管理できる点は同じ目的を
  達成しています)。
