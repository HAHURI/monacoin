Monacoin Core
=============

セットアップ
---------------------
モナコインコアは元のモナコインクライアントであり、ネットワークのバックボーンを構築します。 デフォルトでは、Monacoinトランザクションの全履歴（現在7 GB以上）をダウンロードして保存します。 コンピューターとネットワーク接続の速度に応じて、同期プロセスには数時間から1日以上かかることがあります。

Monacoin Coreをダウンロードするには、[monacoin.org](https://monacoin.org)にアクセスしてください。

Running
---------------------
以下は、ネイティブプラットフォームでMonacoin Coreを実行する方法に関する役立つ注意事項です。

### Unix

ファイルをディレクトリに解凍して実行します：

- `bin/monacoin-qt` (GUI) or
- `bin/monacoind` (headless)

### Windows

ファイルをディレクトリに展開し、monacoin-qt.exeを実行します。

### macOS

Monacoin Coreをアプリケーションフォルダーにドラッグし、Monacoin Coreを実行します。

### 助けが必要？

* ヘルプと詳細については、[Monacoin Wiki](https://monacoin.info/)のドキュメントを参照してください。
* Freenodeの[#monacoin](http://webchat.freenode.net?channels=monacoin)で助けを求めてください。 IRCクライアントがない場合は、[webchat here](http://webchat.freenode.net?channels=monacoin)を使用してください。
* [MonacoinTalk](https://monacointalk.io/)フォーラムで助けを求めてください。

Building
---------------------
以下は、ネイティブプラットフォームでMonacoin Coreをビルドする方法に関する開発者向けのメモです。 完全なガイドではありませんが、必要なライブラリ、コンパイルフラグなどに関するメモが含まれています。

- [Dependencies](dependencies.md)
- [macOS Build Notes](build-osx.md)
- [Unix Build Notes](build-unix.md)
- [Windows Build Notes](build-windows.md)
- [OpenBSD Build Notes](build-openbsd.md)
- [NetBSD Build Notes](build-netbsd.md)
- [Gitian Building Guide](gitian-building.md)

Development
---------------------
The Monacoin repo's [root README](/README.md) contains relevant information on the development process and automated testing.

- [Developer Notes](developer-notes.md)
- [Release Notes](release-notes.md)
- [Release Process](release-process.md)
- [Translation Process](translation_process.md)
- [Translation Strings Policy](translation_strings_policy.md)
- [Travis CI](travis-ci.md)
- [Unauthenticated REST Interface](REST-interface.md)
- [Shared Libraries](shared-libraries.md)
- [BIPS](bips.md)
- [Dnsseed Policy](dnsseed-policy.md)
- [Benchmarking](benchmarking.md)

### Resources
* Discuss on the [MonacoinTalk](https://monacointalk.io/) forums.
* Discuss general Monacoin development on #monacoin-dev on Freenode. If you don't have an IRC client use [webchat here](http://webchat.freenode.net/?channels=monacoin-dev).

### Miscellaneous
- [Assets Attribution](assets-attribution.md)
- [Files](files.md)
- [Fuzz-testing](fuzzing.md)
- [Reduce Traffic](reduce-traffic.md)
- [Tor Support](tor.md)
- [Init Scripts (systemd/upstart/openrc)](init.md)
- [ZMQ](zmq.md)

License
---------------------
Distributed under the [MIT software license](/COPYING).
This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](https://www.openssl.org/). This product includes
cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.
