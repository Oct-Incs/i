# Yokai & Ancient Beasts — 妖異獣譚 (RimWorld MOD)

RimWorld **1.6** 向けの XML-only MOD です。自然発生する敵生物(野生の危険生物)を7種追加します。
NPCファクション/キャラクターは追加しません — 純粋なワイルドライフ/モンスター追加MODです。

全てのdefNameには他MODとの衝突を避けるため `YB_` プレフィックスを付けています(実機テストで
他の動物追加MODと `Meat_Mammoth` 等の名前が衝突することが判明したため)。

## 追加される生物

| 生物 | defName | 系統 | 危険度目安(combatPower) | 生態 |
|---|---|---|---|---|
| アダマンタイト | `YB_AdamantiteBeast` | アダマンタイト | 260 (最上位) | 生きた金属の毛「スカイスチール」を纏う頂点捕食者。極めて稀 |
| 竜人 | `YB_Dragonkin` | 稀種 | 200 | 秘境にごく稀に現れる鱗の人型。伝説級 |
| マンモス | `YB_Mammoth` | 古代種 | 180 | 凍土を歩く古代の巨獣。踏み潰し攻撃が強力 |
| 鬼人 | `YB_Oni` | 稀種 | 150 | 山野に潜む怪力の巨人 |
| 猫又 | `YB_Nekomata` | 稀種 | 85 | 森に潜む俊敏な化け猫 |
| 妖狐 | `YB_Youko` | 稀種 | 80 | 荒野を駆ける最速クラスの化け狐 |
| ニホンオオカミ | `YB_JapaneseWolf` | 古代種 | 70 | 森に群れる俊敏な捕食獣 |

いずれも `trainability: None` / `Wildness` 高めで、飼い慣らしを前提としない「純粋な野生の脅威」として設計しています。

## 素材システム(毛・革・肉)

各生物を討伐すると **肉** と **生の毛皮** が手に入ります。生の毛皮は `HandTailoringBench`
(手作業裁縫台)/ `ElectricTailoringBench`(電動裁縫台)または `CraftingSpot`(簡易クラフト場)で
加工すると、通常のあらゆる革製レシピに使える **加工革** になります。

毛皮・革は種族ごとではなく、以下の3系統で共有されます(ユーザー指定仕様どおり):

| 系統 | 対象生物 | 生の毛皮 | 加工革 |
|---|---|---|---|
| 古代種 | マンモス / ニホンオオカミ | `YB_RawFur_AncientBeast`(古代種の剛毛) | `YB_Leather_AncientBeast`(古代種の厚革) |
| 稀種 | 猫又 / 妖狐 / 鬼人 / 竜人 | `YB_RawFur_RareSpecies`(稀種の輝毛) | `YB_Leather_RareSpecies`(稀種の輝革) |
| アダマンタイト | アダマンタイト | `YB_RawFur_Adamantite`(スカイスチール) | `YB_Leather_Adamantite`(アダマンタイトの硬革) |

肉は生物ごとに個別です。ただし猫又・妖狐・鬼人・竜人の4種(人型の稀種)はバニラの `Meat_Human`
(人肉)を共有します。マンモス(`YB_Meat_Mammoth`)・ニホンオオカミ(`YB_Meat_JapaneseWolf`)・
アダマンタイト(`YB_Meat_Adamantite`)はそれぞれ専用の肉を持ち、`OrganicProductBase` を継承した
通常の生肉として腐敗・食中毒判定などバニラと同じ挙動をします。

加工レシピ(`Defs/RecipeDefs/Recipes_TanHides.xml`):
- `YB_TanAncientBeastHide`: 古代種の剛毛 x2 → 古代種の厚革 x1
- `YB_TanRareSpeciesHide`: 稀種の輝毛 x2 → 稀種の輝革 x1
- `YB_RefineAdamantiteHide`: スカイスチール x2 → アダマンタイトの硬革 x1

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

## 実機テストで判明した1.6の変更点(修正済み)

初回リリース後、実際のPlayer.logで以下のエラーが確認され、修正しました:

- `<race><wildness>` は1.6で廃止され、`<statBases><Wildness>`(Stat化・大文字始まり)に変更する必要がある
- `<race><maxWithSameParentInGrid>` は存在しないフィールドのため削除
- 肉アイテムの共有アブストラクト `MeatBase` は存在しない。`ParentName="OrganicProductBase"` を継承し、
  `ingestible` / `comps`(Rottable)/ `thingCategories` を自前で定義する必要がある
- 裁縫台のdefNameは `TableTailor` ではなく `HandTailoringBench` / `ElectricTailoringBench`
- 他MOD(動物追加系)と `Meat_Mammoth` 等のdefNameが衝突したため、全カスタムdefNameに `YB_` を付与

## プレースホルダー画像について

`Textures/` 以下の画像はすべて Pillow で自動生成した簡易シルエットです。実際のドット絵・イラストに
差し替える場合は、同じファイル名・同じフォルダ構成のまま上書きしてください(128x128 or 160x160、
透過PNG。`_south` / `_north` / `_east` の3方向、west は east が自動反転されて使われます)。

- `Textures/Things/Pawn/Animal/YB_<生物名>/YB_<生物名>_{south,north,east}.png`
- `Textures/Things/Item/Resource/Leather/YB_{RawFur,Leather}_<系統名>.png`
- `Textures/Things/Item/Resource/Meat/YB_Meat_<生物名>.png`
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
- 大量のMODを導入している環境では、他MODとの間で意図しない相性問題が起こる可能性があります。
  導入後にエラーが出た場合はログ内容を教えてください。
