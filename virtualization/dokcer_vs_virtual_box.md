# Docker と VirtualBox の違い

VirtualBox は OS をエミュレートする。一方で Docker はホストOSのカーネルを使うため、OS をエミュレートしない。
[Docker入門（第一回）～Dockerとは何か、何が良いのか～ \| さくらのナレッジ](https://knowledge.sakura.ad.jp/13265/) の `Dockerとは何か、何が良いのか` の図のイメージ。

Docker(コンテナ技術) が優れている点
- パフォーマンス
  - OS をエミュレートしないためリソースが軽量
- 互換性・移植容易性
  - Docker image あれば、どんなホストOS上でも動くため移植が容易

VirtualBox(仮想マシン技術) が優れている点
- セキュリティ
  - ホストOSから完全に分離されているため、ホストOSが危機に瀕しても、Docker の場合よりも影響を受けにくい
  - しかし、ハイパーバイザーが単一障害点なので、ハイパーバイザーが障害を起こすと全ての仮想マシンに障害が起こる(Docker には単一障害点はない)

## 参考
- [virtualization](https://www.comparitech.com/net-admin/docker-vs-virtual-machines/#:~:text=Containers%20share%20operating%20systems%20whereas,resources%20of%20a%20virtual%20machine.)
- [Docker入門（第一回）～Dockerとは何か、何が良いのか～ \| さくらのナレッジ](https://knowledge.sakura.ad.jp/13265/)
