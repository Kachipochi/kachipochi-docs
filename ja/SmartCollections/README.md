# SmartCollections — ドキュメント
![logo](/SmartCollections/images/SmartCollectionsLogo.png)

**パブリッシャー:** Kachipochi  
**対応エンジンバージョン:** Unreal Engine 5.5、5.6、5.7  
**プラグインタイプ:** エディタープラグイン（ランタイムコンポーネントなし）

---

## 概要

SmartCollections は、コンテンツブラウザにスマートなファイルベースのコレクションシステムを追加する Unreal Engine エディタープラグインです。コレクションはプレーンテキストの `.smc` ファイル（JSON 形式）として保存され、プロジェクトと一緒にバージョン管理できます。

主な機能：
- コンテンツブラウザ内で名前付きコレクションを直接作成・管理
- コレクションは `.smc` ファイルとして永続化 — ソースコントロール経由で共有可能
- コンテンツブラウザパネルでコレクションによるアセットのフィルタリング
- 自動化ワークフロー向け Python スクリプトインターフェース

---

## 動作要件

- Unreal Engine 5.5 以降
- 組み込みの **ContentBrowserFileDataSource** プラグインが有効になっていること
  （UE 5.5 以降ではデフォルトで有効）

---

## インストール

1. `SmartCollections` フォルダをプロジェクトの `Plugins/` ディレクトリ、またはグローバルインストールの場合はエンジンの `Plugins/Marketplace/` ディレクトリにコピーします。
   （Fab から直接インストールした場合は、これらの手順は不要です。）
2. Unreal Editor でプロジェクトを開きます。
3. **Edit → Plugins** から **SmartCollections** を検索し、有効化します。
4. 求められたらエディターを再起動します。

---

## 使い方

![OpenMenu](/SmartCollections/images/OpenMenu.png)

### SmartCollections パネルを開く

プラグインを有効化した後、以下の手順でパネルを開きます：

**Tools → Kachipochi → SmartCollections**

パネルはエディター内のドッキング可能なタブとして表示されます。

![NewCollection](/SmartCollections/images/NewCollection.png)
![NewCollection](/SmartCollections/images/NewCollection_2.png)

### コレクションの作成

1. SmartCollections パネルの **+** ボタンをクリックします。
2. 新しいコレクションの名前を入力します。
3. プロジェクトのコンテンツフォルダ内に `.smc` ファイルが作成されます。

![AddSmartCollection](/SmartCollections/images/AddSmartCollection.png)

### コレクションへのアセット追加

1. コンテンツブラウザで 1 つ以上のアセットを選択します。
2. 右クリック → **SmartCollections → Add to Collection → [コレクション名]**

### コレクションによるフィルタリング

SmartCollections パネルでコレクションを選択すると、コンテンツブラウザがそのコレクションに属するアセットのみを表示するようにフィルタリングされます。

---

## Python スクリプト

プラグインは自動化のための Python インターフェースを公開しています。サンプルスクリプトは `Content/Python/smart_collections_examples.py` に用意されています。

```python
import unreal

# すべてのコレクションを取得
collections = unreal.SmartCollectionsLibrary.get_all_collections()

# アセットをコレクションに追加
unreal.SmartCollectionsLibrary.add_asset_to_collection("/Game/MyAsset", "MyCollection")
```

完全なサンプルについては `Content/Python/smart_collections_examples.py` を参照してください。

---

## スタンダードコレクションと Smart Collections の同期サポート

この同期機能は、既存のスタンダードコレクションからの移行や、スタンダードコレクションと Smart Collections を何らかの理由で共存させる必要がある環境向けに設計されています。

### 使い方

#### Collections Sync パネルを開く

以下の手順でパネルを開きます：  
**Tools → Kachipochi → Collections Sync**

![Collection Sync Menu Image](/SmartCollections/images/CollectionSyncMenu.png)

#### Pull または Push を実行

![CollectionSyncWindow](/SmartCollections/images/CollectionSyncWindow.png)

**Pull（UE コレクション → Smart Collections）**  
UE の組み込みコレクションを Smart Collections にインポートします。同名の Smart Collection が存在する場合はアセットが追記され、存在しない場合は新しいものが作成されます。UE の親子階層も再現されますが、既存の Smart Collection に設定済みの親は上書きされません。追記のみ行うため既存エントリは失われず、変更がない場合は「既に最新」としてスキップされるため、繰り返し実行しても安全です。

**Push（Smart Collections → UE コレクション）**  
逆方向：Smart Collections を UE の組み込みコレクションにエクスポートします。同名のコレクションには新しいアセットが追加され、存在しないものは作成されます。Smart Collections の階層も UE 側に再現されます。なお、UE コレクションはフォルダをサポートしていないため、フォルダはスキップされます（スキップ数は表示されます）。この方向も追記的かつ冪等です。  
いずれの場合も、ダイアログに処理結果（作成数 / 更新数 / スキップ数）が表示されます。

---

## サポート

ご質問・バグ報告は以下までお問い合わせください：  
**Kachipochi** — FAB 製品ページのサポートリンクから。
