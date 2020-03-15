macOSビルド手順とメモ
====================================
このガイドのコマンドは、ターミナルアプリケーションで実行する必要があります。
組み込みのものは`/Applications/Utilities/Terminal.app`にあります。

準備
-----------
macOSコマンドラインツールをインストールします。

```bash
xcode-select --install
```

ポップアップが表示されたら、「インストール」をクリックします。

次に、[Homebrew](https://brew.sh)をインストールします。

依存関係
----------------------

```bash
brew install automake berkeley-db4 libtool boost miniupnpc openssl pkg-config protobuf python qt libevent qrencode
```

完全な概要については、[dependencies.md](dependencies.md)を参照してください。

`make deploy`（.dmg / optional）でディスクイメージをビルドする場合は、RSVGが必要です

```bash
brew install librsvg
```

Berkeley DB
-----------
Berkeley DB 4.8を使用することをお勧めします。
自分でビルドする必要がある場合は、[contrib /に含まれるインストールスクリプト](/contrib/install_db4.sh)を使用できます。
以下のようにします

```shell
./contrib/install_db4.sh .
```

リポジトリのrootから。

**注意**：ウォレットが有効になっている場合にのみ、Berkeley DBが必要です（以下の* Disable-Walletモード*セクションを参照）。

モナコインコアの構築
------------------------

1. Monacoin Coreソースコードのクローンを作成し、`monacoin`にcdします

    ```bash
    git clone https://github.com/monacoinproject/monacoin
    cd monacoin
    ```

2. モナコインコアのビルド：

    ヘッドレスMonacoin CoreバイナリとGUI（Qtが見つかった場合）を構成およびビルドします。
    
    `--without-gui`を設定してGUIビルドを無効にすることができます。
    ```bash
    ./autogen.sh
    ./configure
    make
    ```

3. 単体テストをビルドして実行することをお勧めします。

    ```bash
    make check
    ```

4. .appを含む.dmgを作成することもできます
    バンドル（オプション）：
    ```bash
    make deploy
    ```

5. ユーザーディレクトリへのインストール（オプション）：

    ```bash
    make install
    ```
    or
    ```bash
    cd ~/monacoin/src
    cp monacoind /usr/local/bin/
    cp monacoin-cli /usr/local/bin/
    ```

Running
-------

Monacoin Coreが `./src/monacoind`で利用可能になりました。

実行する前に、RPC構成ファイルを作成することをお勧めします。

```bash
echo -e "rpcuser=monacoinrpc\nrpcpassword=$(xxd -l 16 -p /dev/urandom)" > "/Users/${USER}/Library/Application Support/Monacoin/monacoin.conf"

chmod 600 "/Users/${USER}/Library/Application Support/Monacoin/monacoin.conf"
```

初めてmonacoindを実行すると、ブロックチェーンのダウンロードが開始されます。 このプロセスには数時間かかる場合があります。

debug.logファイルを見ると、ダウンロードプロセスを監視できます。

```bash
tail -f $HOME/Library/Application\ Support/Monacoin/debug.log
```

その他のコマンド：
-------

```bash
./src/monacoind -daemon # Starts the monacoin daemon.
./src/monacoin-cli --help # Outputs a list of command-line options.
./src/monacoin-cli help # Outputs a list of RPC commands when the daemon is running.
```

ノート
-----

* 64ビットIntelプロセッサでのみ、OS X 10.10 YosemiteからmacOS 10.13 High Sierraでテスト済み。
* ダウンロードしたQtバイナリを使用したビルドは公式にはサポートされていません。[#7714](https://github.com/bitcoin/bitcoin/issues/7714)
