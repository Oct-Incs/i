# Yokai & Ancient Beasts — 妖異獣譚 (RimWorld MOD)

RimWorld 1.5 向けの XML-only MOD です。自然発生する敵生物(野生の危険生物)を7種追加します。
NPCファクション/キャラクターは追加しません — 純粋なワイルドライフ/モンスター追加MODです。

## 追加される生物

| 生物 | 系統 | 危険度目安(combatPower) | 生態 |
|---|---|---|---|
| アダマンタイト (`AdamantiteBeast`) | アダマンタイト | 260 (最上位) | 生きた金属の毛「スカイスチール」を纏う頂点捕食者。極めて稀 |
| 竜人 (`Dragonkin`) | 稀種 | 200 | 秘境にごく稀に現れる鱗の人型。伝説級 |
| マンモス (`Mammoth`) | 古代種 | 180 | 凍土を歩く古代の巨獣。踏み潰し攻撃が強力 |
| 鬼人 (`Oni`) | 稀種 | 150 | 山野に潜む怪力の巨人 |
| 猫又 (`Nekomata`) | 稀種 | 85 | 森に潜む俊敏な化け猫 |
| 妖狐 (`Youko`) | 稀種 | 80 | 荒野を駆ける最速クラスの化け狐 |
| ニホンオオカミ (`JapaneseWolf`) | 古代種 | 70 | 森に群れる俊敏な捕食獣 |

いずれも `trainability: None` / `wildness` 高めで、飼い慣らしを前提としない「純粋な野生の脅威」として設計しています。

## 素材システム(毛・革・肉)

各生物を討伐すると **肉** と **生の毛皮** が手に入ります。生の毛皮は `TableTailor`(裁縫台)または
`CraftingSpot`(簡易クラフト場)で加工すると、通常のあらゆる革製レシピに使える **加工革** になります。

毛皮・革は種族ごとではなく、以下の3系統で共有されます(ユーザー指定仕様どおり):

| 系統 | 対象生物 | 生の毛皮 | 加工革 |
|---|---|---|---|
| 古代種 | マンモス / ニホンオオカミ | `RawFur_AncientBeast`(古代種の剛毛) | `Leather_AncientBeast`(古代種の厚革) |
| 稀種 | 猫又 / 妖狐 / 鬼人 / 竜人 | `RawFur_RareSpecies`(稀種の輝毛) | `Leather_RareSpecies`(稀種の輝革) |
| アダマンタイト | アダマンタイト | `RawFur_Adamantite`(スカイスチール) | `Leather_Adamantite`(アダマンタイトの硬革) |

肉は生物ごとに個別です。ただし猫又・妖狐・鬼人・竜人の4種(人型の稀種)はバニラの `Meat_Human`(人肉)を
共有します。マンモス(`Meat_Mammoth`)・ニホンオオカミ(`Meat_JapaneseWolf`)・アダマンタイト
(`Meat_Adamantite`)はそれぞれ専用の肉を持ちます。

加工レシピ(`Defs/RecipeDefs/Recipes_TanHides.xml`):
- `TanAncientBeastHide`: 古代種の剛毛 x2 → 古代種の厚革 x1
- `TanRareSpeciesHide`: 稀種の輝毛 x2 → 稀種の輝革 x1
- `RefineAdamantiteHide`: スカイスチール x2 → アダマンタイトの硬革 x1

## 自然出現バイオーム

`Patches/Patch_BiomeWildSpawn.xml` で既存バイオームの `animalCommonalities` に追加しています
(値が小さいほど出現頻度が低い、Adamantite/Dragonkinは特に稀少):

- ニホンオオカミ: TemperateForest / BorealForest / Tundra
- マンモス: Tundra / BorealForest / ExtremeDesert
- 猫又: TemperateForest / TemperateSwamp / TropicalRainforest
- 妖狐: TemperateForest / AridShrubland / Desert
- 鬼人: AridShrubland / ExtremeDesert / Tundra
- 竜人: TropicalRainforest / ExtremeDesert / Tundra / BorealForest (すべて低頻度)
- アダマンタイト: ExtremeDesert / Tundra (極めて低頻度)

## ファイル構成

```
YokaiBeasts/
├── About/
│   ├── About.xml
│   └── Preview.png                      (プレースホルダー)
├── Defs/
│   ├── ThingDefs_Races/
│   │   ├── Races_Beasts.xml             … マンモス/ニホンオオカミ/アダマンタイト
│   │   └── Races_Yokai.xml              … 猫又/妖狐/鬼人/竜人
│   ├── ThingDefs_Items/
│   │   ├── Materials_Furs.xml           … 生毛皮・加工革(3系統 x2)
│   │   └── Meats.xml                    … 専用の肉(マンモス/ニホンオオカミ/アダマンタイト)
│   ├── RecipeDefs/Recipes_TanHides.xml  … なめし加工レシピ
│   └── PawnKindDefs/PawnKinds_Monsters.xml … 7種の個体設定(成長段階ごとの見た目)
├── Patches/
│   └── Patch_BiomeWildSpawn.xml         … 既存バイオームへの野生出現テーブル追加
└── Textures/                             … プレースホルダー画像(後述)
```

## インストール方法

1. `YokaiBeasts` フォルダごと RimWorld の `Mods` フォルダにコピーします。
   - Steam版の例: `Steam/steamapps/common/RimWorld/Mods/YokaiBeasts`
2. RimWorld を起動し、Mod一覧から `Yokai & Ancient Beasts - 妖異獣譚` を有効化してゲームを再起動。
3. **開発者モード**でゲームを開始し、ログに赤いエラー(XMLロードエラー)が出ていないか確認してください。
   人型(`body: Human`)の動物種(猫又・妖狐・鬼人・竜人)や、`animalCommonalities` パッチなど、
   実際のRimWorld環境での動作確認はできていない部分があるため、細部の調整が必要になる可能性があります。
   エラーが出た場合はログ内容を教えてください、対応します。

## プレースホルダー画像について

`Textures/` 以下の画像はすべて Pillow で自動生成した簡易シルエットです。実際のドット絵・イラストに
差し替える場合は、同じファイル名・同じフォルダ構成のまま上書きしてください(128x128 or 160x160、
透過PNG。`_south` / `_north` / `_east` の3方向、west は east が自動反転されて使われます)。

- `Textures/Things/Pawn/Animal/<生物名>/<生物名>_{south,north,east}.png`
- `Textures/Things/Item/Resource/Leather/{RawFur,Leather}_<系統名>.png`
- `About/Preview.png` — Steam Workshop用プレビュー画像

## カスタマイズしやすいポイント

- **出現頻度/バイオーム**: `Patches/Patch_BiomeWildSpawn.xml` の `<commonality>` 値やバイオーム一覧。
- **戦闘力・危険度**: 各 `Defs/ThingDefs_Races/*.xml` の `statBases` / `tools` / `manhunterOnDamageChance`。
- **素材の価値・防御力**: `Defs/ThingDefs_Items/Materials_Furs.xml` の `statBases` / `stuffProps`。
- **なめし加工の手間**: `Defs/RecipeDefs/Recipes_TanHides.xml` の `workAmount` / `ingredients`。

## 既知の制約

- Royalty/Ideology/Biotech/Anomaly 等のDLC固有バイオームには出現パッチを当てていません(バニラ標準
  バイオームのみ対応)。追加したい場合は `Patches/Patch_BiomeWildSpawn.xml` に同様の
  `PatchOperationConditional` ブロックを追加してください。
- 猫又・妖狐・鬼人・竜人は人型シルエットにするため `body: Human` を流用しつつ、知能は通常の動物と
  同じ `Animal`(会話・装備・派閥なし、純粋な野生生物として行動)としています。
