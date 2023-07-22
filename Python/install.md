# Python 環境構築

- [Python 環境構築](#python-環境構築)
  - [pyenv](#pyenv)
  - [python -m venv .venv](#python--m-venv-venv)

*Python3*の環境構築についてまとめる.
メモ：docker でよくね w → これはこれであってもよいだろ

### pyenv

インストール可能な Python バージョン一覧を見る.

```
pyenv install --list
```

指定した Python バージョンをインストールする.[^pyenv_win_anaconda]
[^pyenv_win_anaconda]:pyenv-win では anaconda がサポートされていない.

```
pyenv install 3.8.4
pyenv install 3.6.0
```

インストール済みの Python バージョン一覧を見る.

```
pyenv versions
```

global コマンドで Python バージョンを切り替える.

```

pyenv global 3.8.4

```

local コマンドでカレントディレクトリに`.python-version`ファイルを作成する.
これにより, そのディレクトリでは指定した Python バージョンが利用されるようになる.
これは global 設定に優先する.

```

pyenv local 3.6.0

```

### python -m venv .venv

```

```
