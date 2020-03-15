FreeBSDビルドガイド
======================
(updated for FreeBSD 11.1)

このガイドでは、FreeBSDでmonacoindおよびコマンドラインユーティリティを構築する方法について説明します。

このガイドには、GUIの構築手順は含まれていません。

## Preparation

pkgを介してrootとしてインストールできる次の依存関係が必要になります。

```
pkg install autoconf automake boost-libs git gmake libevent libtool openssl pkgconf
```

ウォレットの場合（オプション）：
```
./contrib/install_db4.sh `pwd`
export BDB_PREFIX="$PWD/db4"
```

完全な概要については、[dependencies.md](dependencies.md)を参照してください。

ソースコードをダウンロードします。
```
git clone https://github.com/monacoinproject/monacoin
```

## モナコインコアの構築

**重要**： `gmake`を使用します（GNU以外の` make`はエラーで終了します）。

```
./autogen.sh

./configure                  # to build with wallet OR
./configure --disable-wallet # to build without wallet

gmake
```

*デバッグに関する注意*：デフォルトでインストールされる `gdb`のバージョンは[古代かつ有害と見なされます](https://wiki.freebsd.org/GdbRetirement)です。
バックスレッドを取得するためでなく、マルチスレッドC ++プログラムのデバッグには適していません。 
パッケージ `gdb`をインストールし、バージョン管理されたgdbコマンド（例：` gdb7111`）を使用してください。
