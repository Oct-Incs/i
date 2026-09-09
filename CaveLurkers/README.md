# Cave Lurkers — 洞窟の潜伏者 (RimWorld MOD)

RimWorld 1.5 向けの XML-only MOD です。自然発生する敵生物「ケイブ・ラーカー」と、
それに関連する中立ファクション「洞窟ハンター団」を追加します。C#/DLL は使用していないため、
既存セーブへの追加・削除も比較的安全です。

## 追加される内容

- **ケイブ・ラーカー (`CaveLurker`)**: 洞窟・寒冷地に潜む夜行性の捕食獣。以下のバイオームに低〜中頻度で自然出現します。
  - `TemperateForest` / `TemperateSwamp` / `BorealForest` / `Tundra` / `AridShrubland` / `Desert` / `ExtremeDesert` / `TropicalRainforest` / `TropicalSwamp`
  - マンハンター化しやすい（`manhunterOnDamageChance = 0.40`）ため、不用意な攻撃は危険です。
  - 討伐すると `Leather_CaveLurker`（ラーカーハイド）をドロップします。これは通常の革と同じ
    `StuffCategory: Leathery` に属するため、**既存のあらゆる革製装備レシピでそのまま使用可能**です。
- **洞窟ハンター団 (`LurkerHunterClan`)**: ラーカーハイド装備をまとう中立の狩猟部族ファクション。
  非戦闘的（`canSiege=false`, `canStageAttacks=false`）で、来訪者グループや交易キャラバン
  (`LurkerHunterCaravanTrader`) として登場します。ゲーム開始時に必ず1勢力生成されます
  (`requiredCountAtGameStart=1`)。

## ファイル構成

```
CaveLurkers/
├── About/
│   ├── About.xml
│   └── Preview.png              (プレースホルダー)
├── Defs/
│   ├── ThingDefs_Races/Race_CaveLurker.xml     … 動物種本体
│   ├── ThingDefs_Items/Leather_CaveLurker.xml  … 毛皮アイテム
│   ├── PawnKindDefs/PawnKind_CaveLurker.xml    … 動物の個体設定
│   ├── PawnKindDefs/PawnKind_LurkerHunter.xml  … ハンターNPCの個体設定
│   ├── FactionDefs/Faction_LurkerHunterClan.xml
│   └── TraderKindDefs/Trader_LurkerHunterCaravan.xml
├── Patches/
│   └── Patch_BiomeWildSpawn.xml  … 既存バイオームへの野生出現テーブル追加
└── Textures/                     … プレースホルダー画像（後述）
```

## インストール方法

1. `CaveLurkers` フォルダごと RimWorld の `Mods` フォルダにコピーします。
   - Steam版の例: `Steam/steamapps/common/RimWorld/Mods/CaveLurkers`
   - ローカルMODとして使う場合は各OSのユーザーMODフォルダでも可。
2. RimWorld を起動し、Mod一覧から `Cave Lurkers - 洞窟の潜伏者` を有効化してゲームを再起動。
3. **開発者モード**でゲームを開始し、ログに赤いエラー（XMLロードエラー）が出ていないか確認してください。
   本MODは実際のRimWorld環境で動作確認できていないため、フィールド名の細部で調整が必要になる
   可能性があります。エラーが出た場合はログ内容を教えてください、対応します。

## プレースホルダー画像について

`Textures/` 以下の画像はすべて Pillow で自動生成した簡易プレースホルダーです（シルエット/アイコン）。
実際のドット絵・イラストに差し替える場合は、同じファイル名・同じフォルダ構成のまま上書きしてください。

- `Textures/Things/Pawn/Animal/CaveLurker/CaveLurker_{south,north,east}.png` — ケイブ・ラーカー本体
  （128x128、透過PNG。west は east が自動反転されて使われます）
- `Textures/Things/Item/Resource/Leather/Leather_CaveLurker.png` — ラーカーハイドのアイコン
- `Textures/World/FactionIcons/LurkerHunterClan.png` — ファクションアイコン
- `About/Preview.png` — Steam Workshop用プレビュー画像

## カスタマイズしやすいポイント

- **出現頻度/バイオーム**: `Patches/Patch_BiomeWildSpawn.xml` の `<commonality>` 値やバイオーム一覧を編集。
- **戦闘力・危険度**: `Defs/ThingDefs_Races/Race_CaveLurker.xml` の `statBases` / `tools` / `manhunterOnDamageChance`。
- **ファクションの友好度・出現頻度**: `Defs/FactionDefs/Faction_LurkerHunterClan.xml` の
  `settlementGenerationWeight` / `pawnGroupMakers`。
- **ハンターの装備傾向**: `Defs/PawnKindDefs/PawnKind_LurkerHunter.xml` の `apparelTags` / `weaponTags` /
  `apparelMoney` / `weaponMoney`。

## 既知の制約

- Royalty/Ideology/Biotech/Anomaly 等のDLC固有バイオームには出現パッチを当てていません（バニラ標準バイオームのみ対応）。
  追加したい場合は `Patches/Patch_BiomeWildSpawn.xml` に同様の `PatchOperationConditional` ブロックを追加してください。
- 会話イベント・クエスト連携（洞窟ハンター団が特定のクエストを出す等）は未実装です。次のステップとして
  `QuestScriptDef` を追加すればクエスト連携も可能です。
