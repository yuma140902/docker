# docker

## dev-arch

archlinux をベースに、開発用のツールと [yuma140902/dotfiles-public](https://github.com/yuma140902/dotfiles-public) をインストールした開発用のコンテナイメージ。docker pull して docker run しても良いし、このイメージをベースに開発用イメージを作っても良い。

### タグ

- c
	- C/C++ 用の開発環境が用意されているコンテナイメージ
- node
	- Node.js 用の開発環境が用意されているコンテナイメージ
- python
	- Python 用の開発環境が用意されているコンテナイメージ
- rust
	- Rust 用の開発環境が用意されているコンテナイメージ
- all
	- 上記すべての機能が有効化されているコンテナイメージ
- base
	- 上記すべての機能が無効化されている、共通機能だけが有効になっているコンテナイメージ

### 実行方法

```sh
docker run --rm -it ghcr.io/yuma140902/dev-arch:all
```
