OpenBSDビルドガイド
======================
(updated for OpenBSD 6.3)

このガイドでは、OpenBSDでmonacoindおよびコマンドラインユーティリティを構築する方法について説明します。

OpenBSDはサーバーOSとして最も一般的に使用されているため、このガイドにはGUIの構築手順は含まれていません。

準備
-------------

rootとして次を実行して、ビルドの基本依存関係をインストールします。

```bash
pkg_add git gmake libevent libtool boost
pkg_add autoconf # (select highest version, e.g. 2.69)
pkg_add automake # (select highest version, e.g. 1.15)
pkg_add python # (select highest version, e.g. 3.6)

git clone https://github.com/monacoinproject/monacoin.git
```

完全な概要については、[dependencies.md](dependencies.md)を参照してください。


**重要**：OpenBSD 6.2以降では、C++11をサポートするclangコンパイラーがベースイメージの一部であり、ビルド中に、このコンパイラーが古代のg++ 4.2.1ではなく使用されていることを確認する必要があります。 これは、設定コマンドに`CC=cc CXX=c++` を追加することで実行されます。 同じ実行可能ファイル内で異なるコンパイラを混在させると、リンカーエラーが発生します。

### BerkeleyDBの構築

BerkeleyDBは、ウォレット機能にのみ必要です。
これをスキップするには、 `--disable-wallet`を`./configure`に渡し、次のセクションにスキップします。

Berkeley DB 4.8を使用することをお勧めします。上記のboostと同じ理由で(g++/libstd++ の非互換性)、ポートからBerkeleyDBライブラリを使用することはできません。自分でビルドする必要がある場合は、[contrib /に含まれるインストールスクリプト](/contrib/install_db4.sh)を使用できます。

```shell
./contrib/install_db4.sh `pwd` CC=cc CXX=c++
```
リポジトリのルートから。 次に、次のセクションの `BDB_PREFIX`を設定します。

```shell
export BDB_PREFIX="$PWD/db4"
```

### モナコインコアの構築

**重要**： `gmake`を使用します（GNU以外の` make`はエラーで終了します）。

準備
```bash

# Replace this with the autoconf version that you installed. Include only
# the major and minor parts of the version: use "2.69" for "autoconf-2.69p2".
export AUTOCONF_VERSION=2.69

# Replace this with the automake version that you installed. Include only
# the major and minor parts of the version: use "1.15" for "automake-1.15.1".
export AUTOMAKE_VERSION=1.15

./autogen.sh
```
`BDB_PREFIX`が上記のステップから適切なパスに設定されていることを確認してください。

ウォレットで構成するには：
```bash
./configure --with-gui=no CC=cc CXX=c++ \
    BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"
```

ウォレットなしで構成するには：
```bash
./configure --disable-wallet --with-gui=no CC=cc CXX=c++
```

テストをビルドして実行します。
```bash
gmake # use -jX here for parallelism
gmake check
```

リソース制限
-------------------

ビルドでメモリ不足エラーが発生した場合は、このセクションの手順が役立つ場合があります。

OpenBSDの標準のulimit制限は非常に厳格です

    data(kbytes)         1572864

残念ながら、これはプロジェクトの一部の `.cpp`ファイルをコンパイルするには不十分な場合があります(see issue [#6658](https://github.com/bitcoin/bitcoin/issues/6658)).

ユーザーが `staff`グループに属している場合、制限は次のように上げることができます：

    ulimit -d 3000000

変更は、現在のシェルとそれによって生成されたプロセスにのみ影響します。
システム全体に変更を加えるには、`/etc/login.conf`の`datasize-cur`と`datasize-max`を変更して再起動します。
