
# 組み込み関数 - ネットワーク


!!! 情報 Info
    ルームのプレイヤー全体に、任意のデータを送信します。

    プレイヤーにデータを送信する方法として、カスタムステートとカスタムデータの2種類があります。

    カスタムステートは、新しくルームに入ってきたプレイヤーへ自動的にデータが送信されます。

    カスタムデータは、1回きりの送信を実現するものです。

    それぞれ、以下の関数を利用してデータを送信します。type には任意の文字列を指定できますが、利用目的に応じた適切な名前を選択することが好ましいです。

***

### hsNetSetCustomState(string, string)
`void hsNetSetCustomState(string type, string data)`

あらかじめ type と data を設定しておくことによって、ルームに入場したユーザーに (type, data) を自動で通知する。

### hsNetSendCustomData(string, string)
`void hsNetSendCustomData(string type, string data)`

ルーム内のユーザーに (type, data) を通知する。


### データ受信コールバックメソッド

送信されたデータは、コンポーネントに定義された `OnReceiveCustomState()` と `OnReceiveCustomData()` メソッドで取得できます。引数 id は送信したプレイヤーの識別子です。

```
component CustomDataReceiver
{
    public void OnReceiveCustomState(string id, string type, string data)
    {
    }

    public void OnReceiveCustomData(string id, string type, string data)
    {
    }
}
```

## 音声チャンネル入退出コールバックメソッド

他のユーザーの音声チャンネルへの入退出は`OnVCPlayerJoin()`と`OnVCPlayerLeave()`メソッドで取得できます。引数 ID は送信したプレイヤーの識別子です。
```
component JoinLeaveSample
{
    public void OnVCPlayerJoin(string ID, string Data)
    {
    }

    public void OnVCPlayerLeave(string ID, string Data)
    {
    }
}
```


## マイク許可状態

### hsNetGetMicPermissionState
`int hsNetGetMicPermissionState()`

マイク許可状態を取得します。以下の定数値が返ります。

```
const int HSMicPermissionState_Prompt = 0;		// 確認が必要
const int HSMicPermissionState_Granted = 1;		// 許可
const int HSMicPermissionState_Denied = 2;		// 不許可
```


### コールバックメソッド

以下のメソッドを定義しておくと、マイク状態が変更されたときに呼び出されます。

```
public void OnChangedMicPermissionState(int MicPermissionState)
{
}
```


## SpatiumCode

### hsNetGetSpatiumCode
`string hsNetGetSpatiumCode()`

Sceneファイルに定義されたSpatiumCodeを取得します。
