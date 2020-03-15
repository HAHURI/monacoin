NetBSD構築ガイド
======================
(updated for NetBSD 7.0)

このガイドは、NetBSD上でmonacoindおよびコマンドラインユーティリティを構築する方法を説明しています。

このガイドには、GUIの構築手順は含まれていません。

準備
-------------

次のモジュールが必要になります。これらのモジュールは、pkgsrcまたはpkginを使用してインストールできます。
```
autoconf
automake
boost
db4
git
gmake
libevent
libtool
python27
```

ソースコードをダウンロードします。
```
git clone https://github.com/monacoinproject/monacoin
```

完全な概要については、[dependencies.md](dependencies.md)を参照してください。

### モナコインコアの構築

**重要**： `gmake`を使用します（GNU以外の` make`はエラーで終了します）。

ウォレット付き：
```
./autogen.sh
./configure CPPFLAGS="-I/usr/pkg/include" LDFLAGS="-L/usr/pkg/lib" BOOST_CPPFLAGS="-I/usr/pkg/include" BOOST_LDFLAGS="-L/usr/pkg/lib"
gmake
```

ウォレットなし：
```
./autogen.sh
./configure --disable-wallet CPPFLAGS="-I/usr/pkg/include" LDFLAGS="-L/usr/pkg/lib" BOOST_CPPFLAGS="-I/usr/pkg/include" BOOST_LDFLAGS="-L/usr/pkg/lib"
gmake
```
