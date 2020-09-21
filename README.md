RStudio Server で自動的にプロジェクトが開くようにする裏技
================================================================================

Docker コンテナ [rocker/rstudio](https://hub.docker.com/r/rocker/rstudio) で RStudio Server を起動したとき,
ファイル `~/.rstudio/projects_settings/switch-to-project` に .Rproj ファイルのパスを書き込んでおくと,
次にログインしたとき自動的にそのプロジェクトが開くようになる.

これはシステム用ファイルを書き換える方法であり, 公式に提供されている機能ではない.
RStudio のバージョン等によっては動作しなくなる可能性もあるので利用の際は注意のこと.


このリポジトリでは [`docker-compose.yml`](./docker-compose.yml) の設定でコンテナ起動時コマンドをデフォルトの `/init` から次のようなコマンドに変更している:

```sh
/bin/sh -c 'mkdir -p /home/rstudio/.rstudio/projects_settings \
  && echo /home/rstudio/myproject/myproject.Rproj \
    > /home/rstudio/.rstudio/projects_settings/switch-to-project \
  && chown -R rstudio:rstudio /home/rstudio/.rstudio \
  && /init'
```

`docker-compose up -d` でサービスを起動し, ブラウザで <http://localhost:8787> を開くと
R プロジェクト `/home/rstudio/myproject/myproject.Rproj` が自動的に開かれる.
