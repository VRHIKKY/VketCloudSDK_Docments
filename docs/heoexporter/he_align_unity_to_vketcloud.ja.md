VketCloudではglobal illuminationなどまだ一部対応できていない機能があります。下記の設定をすることでUnity側とVketCloud側の見た目を揃えることができます。

## グローバルイルミネーションについて
VketCloudではリアルタイムのグローバルイルミネーションをサポートしていないので、そちらはライトマップで表現してください。[Unity制作ガイドライン/ライトマップ](./he_UnityGuidelines.ja.md#_6) (UnityとVketCloudで見た目が違う場合、ほとんどはGI周りが原因だと思います)

## SkyboxをNoneにする
Skyboxを使用していないのであればUnityではLight SettingsのSkyboxの色味がglobal illuminationで乗ってしまうのでNoneにしてください  
※リフレクションプローブの撮影でSkyboxを使用する時はオンにしていただいて大丈夫です([リフレクションプローブの作成](./he_ReflectionProbe.md))

<img src="he_image/スクリーンショット 2022-05-13 135911.jpg">

## StandardShaderの設定を変更する
VketCloudの物理ベースレンダリングは、UnityのMediumレベルのものと同じアルゴリズム(GGX)を使用しているので、設定を揃える必要があります。

1. 「Edit/ProjectSettings/Graphics」を開く
<img src="he_image/スクリーンショット 2022-05-13 141258.jpg">
2.  「Tier Settings」のLow、Medium、Highそれぞれの「Use Defaults」のチェックを外す
<img src="he_image/スクリーンショット 2022-05-13 141452.jpg">
3. 「Tier Settings」のLow、Medium、Highそれぞれの「Standard Shader Quality」をすべて「Medium」に変更する
<img src="he_image/スクリーンショット 2022-05-13 141630.jpg">

## カラースペースがリニアになっていることを確認する
「Edit/Project Settings/Player/Other Settings」を開き、Color Spaceが「Linear」になっていることを確認する
<img src="he_image/スクリーンショット 2022-05-13 143143.jpg">