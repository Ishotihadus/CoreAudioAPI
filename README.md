# CoreAudioAPI

艦これクライアント「みおつくし」用に http://whenimbored.xfx.net/2011/01/core-audio-for-net/ を独自に改良しました。

といっても、guidがかかわる関数をすべてrefになおしただけです。

## 使い方

`MMDeviceEnumerator`がデバイスのenmueratorになっているので、このインスタンスを作ってあげます。

    var device_enumerator = new MMDeviceEnumerator();

ここから`GetDefaultAudioEndpoint`でデフォルトのデバイスを得るか、`EnumerateAudioEndPoints`でデバイスの一覧を持ってきた後に好きなものを選びます。

    var device = GetDefaultAudioEndpoint(EDataFlow.eRender, ERole.eMultimedia);

デバイスがいくつかのセッションを持っており、その中にプロセスのセッションがあるので、そのセッションの一覧を取得した後にその中から目的のセッションを選び出します。

    var sessions = device.AudioSessionManager2;
    var session = sessions[hoge];

セッションは`GetSessionInstanceIdentifier`とか`GetProcessID`とかあるので、その辺を使ってほしいものを見つけ出します。

もしセッションが作成されていなくて、セッションの作成を監視したい場合は`sessions`の`OnSessionCreated`イベントを監視してあげるとよいです。セッションが新たに作られた際は、`sessions.RefreshSessions()`で一覧を更新してあげる必要があります。

みおつくしでは、プロセスのボリュームをいじいじしたいので、選び出した`session`の`SimpleAudioVolume`にある`MasterVolume`とか`Mute`とかをいじっています。こいつらは、外部からいじられたことを`session`の`OnSimpleVolumeChanged`で監視できます。

ちなみに、もとのライブラリをそのまま使うとAccess Violationで落ちます。

## バグとか

プロセスのボリュームをいじる目的でしか使ってないので、ほかの目的で使用した際には死ぬかもしれないです。Bug ReportおよびPull Requestお待ちしてます。

MixerProNETとかでも動くんですかね。知らないです。

## 適用ライセンス

もとのライブラリのライセンスに依存します。
