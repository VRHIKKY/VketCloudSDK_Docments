# VKC/HEO コンポーネント 概要

VketCloudSDKには、以下のような種類のコンポーネントが存在します。

- **Setting 項目**: シーンごとに1つの値しか割り当てられないコンポーネント
- **Item 項目**: シーンに複数の値を振り当てられるコンポーネント
- **Node 項目**: Nodeに関するコンポーネント
- **Attribute 項目**: Item/Node/Object等に、属性を与えるコンポーネント
- **Legacy 項目（非推奨）**: 旧バージョンのコンポーネント

```mermaid
classDiagram
    シーン "1" *--> "1" Setting
    シーン "1" *--> "0..N" Item
    Item "1" *--> "0..N" Node: type が Field または Object の場合
    Item "1" o--> "0..N" Attribute
    Node "1" o--> "0..N" Attribute
    
    class Item {
        type
    }
```

## Setting 項目

| 大項目    | コンポーネント名 (-SDK9.X)  | コンポーネント名 (SDK10.X-SDK12.3) | コンポーネント名 (SDK13.X-) |
|-----------|----------------------------|------------------------------------|-----------------------------|
| Basic     | HEO World Settings         | Base Setting                        | VKC Setting Base            |
| Player    | HEO Player                 | Player Setting                      | VKC Setting Player          |
| DespawnHeight    | HEO Despawn Height                 | Despawn Height Setting                      | VKC Setting Despawn Height          |
| Rendering | HEO Rendering              | Rendering Setting                       | VKC Setting Rendering       |
| Camera    | HEO World Settings         | World Camera Setting                | VKC Setting World Camera    |
| Avatars   | HEO World Settings         | Avatar Setting                      | VKC Setting Avatar          |
| My Avatar | HEO World Settings         | My Avatar Setting                   | VKC Setting My Avatar       |
| Spawn     | HEO Spawn                  | HEO Spawn                           | VKC Setting Spawn           |
| Nameplate | HEO Nameplate              | HEO Nameplate                       | VKC Setting Nameplate       |

## Item 項目

Vket Cloudのワールドの１シーンにおいて配置されているモデル、コライダー、サウンド、パーティクルなど様々な構成要素は内部的にはitemとして分類されます。<br>
これらのitemは、VKC Item Field、VKC Item Objectなど、VketCloudSDKによって追加されたコンポーネントを持つゲームオブジェクトを配置・設定することでシーンに出力することが可能です。

itemにはtypeという要素があり、typeによって、そのitemの役割が決まります。<br>
typeは十数種類あり、いくつか例に挙げると、

- fieldタイプのitemであれば、設置された位置を動かせないもの。
- objectタイプのitemであれば、位置を動かしたり、Animationを行えるもの。
- planeタイプのitemであれば、画像を配置できるもの。
- textplaneタイプのitemであれば、文字入力ができるもの。
- cameraタイプのitemであれば、演出目的等で通常のカメラから切り替えるためのもの。
- bgm, se, systemタイプのitemであれば、サウンドの再生に使用するもの。(コンポーネント名はAudio)
- particleタイプのitemであれば、パーティクルの再生に使用するもの。

というように振舞います。

| Item 項目     | コンポーネント名 (-SDK12.3) | コンポーネント名 (SDK13.X-) |
|---------------|-----------------------------|------------------------------|
| Field         | HEO Field                   | VKC Item Field               |
| Object        | HEO Object                  | VKC Item Object              |
| Plane         | HEO Plane                   | VKC Item Plane               |
| TextPlane     | HEO Text Plane              | VKC Item Text Plane           |
| Audio         | HEO Audio                   | VKC Item Audio               |
| Particle      | HEO Particle                | VKC Item Particle            |
| Spot          | HEO Spot                    | VKC Item Spot                |
| Area Collider | HEO Area Collider           | VKC Item Area Collider       |
| Bg Texture    | HEO Background Texture      | VKC Item Background Texture  |
| Activity      | HEO Activity                | VKC Item Activity            |
| Camera        | HEO Camera                  | VKC Item Camera              |

### Item - Fieldについて

fieldタイプのitemは、シーン上の設置された位置から動かせません。そのため、scriptを使用して、fieldタイプのitemを取得して動かそうとしても、仕様上動かせません。<br>
一方で、nodeに対しての操作が豊富に行えたり、Unity上での編集が簡単に行える利点があります。<br>
動かない地形などについては、fieldとして設置するのが推奨されます。<br>

fieldとしてオブジェクトを設置するには、以下の手順を行います：

1. [VKC Item Field](VKCItemField.md)コンポーネントを持つゲームオブジェクトをUnityのシーン上に設置。

2. 設置したゲームオブジェクトの子に3Dモデルやコライダーを配置する。

これによって、Field及びその子要素はビルド時にheoファイルとしてパックされ、シーン上にfieldタイプのitemとして登録されます。<br>
ここで登録されたFieldの子要素のオブジェクトは、後述のNodeとして表現されます。

### Item - Objectについて

fieldと同じく3Dモデルを配置するためのtypeとしてobjectがあります。<br>
objectタイプのitemは、fieldと違って移動、回転、スケールの変更がスクリプトによって変更できたり、さらに、ボーンアニメーションを含むアニメーションが使用できたりします。<br>
ただし、書き出しまでの手順が少し複雑であるため、何度も微調整が必要なものに関してはfieldにしておいた方が編集の面において楽と考えられます。<br>
動くモノや、scriptによる複雑な動作をさせたいモノに関しては、objectとして設置するのが推奨されます。

objectとしてオブジェクトを設置するには、以下の手順を行います：

1. objectとして設定したいゲームオブジェクトを選択（複数選択は不可）

2. VketCloudSDK > [Export Field](../WorldMakingGuide/HEOExporter_Tutorial.md)を行う。

3. heoファイルの保存場所を設定し書き出す。

4. [VKC Item Object](../VKCComponents/VKCItemObject.md)コンポーネントを持つゲームオブジェクトをUnityのシーン上に配置する

5. 書き出したheoを[VKC Item Object](../VKCComponents/VKCItemObject.md)に設定する。

これによって、ビルド時に設定されたheoファイルが任意の場所にコピーされ、シーン上にobjectタイプのitemとして登録されます。

※また、わかりづらい要素として、Avatarのボーンに対して追従するモノについてもObjectという名称になっています。
[Action Trigger](VKCAttributeActionTrigger.md)で設定できる[Show/HideObject](../Actions/Object/ShowHideObject.md)のObjectはこのAvatarに設定できるObjectのことを指します。

## Node 項目

Vket Cloudにて、前述のField及びObjectは.heoという独自の3Dモデルの規格によって表現されます。<br>
大きな特徴として、Unity上での子オブジェクトは.heoにおいては**Node**として扱われ、Nodeに関連するコンポーネントやHeliScript関数の対象となります。

なお、重要な点として**nodeは階層構造を持っていません。**<br>
Unity上でオブジェクトが階層構造を持っていたとしても、heoとして書き出した後は一列に並んでいます。<br>
Unity上では親だったGameObjectを選択して表示非表示などの動作を行ったとしても、Vket Cloud上では親子関係はなくなっているため、したがってもともと子だった要素に対して表示非表示は動作しません。

| Node 項目          | コンポーネント名 (-SDK12.3) | コンポーネント名 (SDK13.X-)       |
|--------------------|-----------------------------|-----------------------------------|
| AlphaAnim          | -                           | VKC Node Alpha Animation          |
| Animation          | HEO Animation               | VKC Node Rotate Animation         |
| Rotate Animation   | HEO Rotate Animation        | 廃止(VKC Node Rotate Animationと統合) |
| Collider           | HEO Collider                | VKC Node Collider                 |
| Cylinder Collider  | HEO Cylinder Collider       | VKC Node Cylinder Collider        |
| IBLCubeMap         | -                           | -                                 |
| Info               | HEO Info                    | VKC Node Blend Shape Translator   |
| LODLevel           | HEO LOD Level               | VKC Node LOD Level                |
| Mesh Collider      | HEO Mesh Collider           | VKC Node Mesh Collider            |
| Mirror             | HEO Mirror                  | VKC Node Mirror                   |
| Reflection Probe   | -                           | -                                 |
| ObjectType         | HEO Object Type             | VKC Node Reflection Probe Type    |
| Shadow             | HEO Shadow                  | VKC Node Shadow                   |
| UVScroll           | HEO UV Scroller             | VKC Node UV Scroller              |
| Video              | HEO Video Trigger           | VKC Node Video Trigger            |

## Attribute 項目

| Attribute 項目 | コンポーネント名 (-SDK12.3) | コンポーネント名 (SDK13.X-)       |
|----------------|-----------------------------|-----------------------------------|
| Action Trigger  | HEO Action Trigger          | VKC Attribute Action Trigger      |
| Property        | HEO Property                | VKC Attribute Property            |
| Script          | HEO Script                  | VKC Attribute Script              |
| Show Flag       | HEO Show Flag               | VKC Attribute Show Flag           |
| Clickable UI    | -                           | VKC Attribute Clickable UI        |
| Clickguide      | HEO Clickguide              | VKC Attribute Click Guide         |

## Legacy 項目

- HEO World Setting
- HEO Despawn Height
- HEO Player
- HEO Redire
- HEO Selecting Object
- HEO Text Preview
