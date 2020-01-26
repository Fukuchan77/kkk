# jupyter/datascience-notebookでPythonデータ解析環境の構築

1. Dockerをインストールする。

2. Dockerイメージを取得する。

```
$ docker pull jupyter/datascience-notebook
```

3. 設定するパスワードのハッシュ文字列の取得

 - dockerコンテナのbash環境に入ります。
```
$ docker run -it --rm jupyter/datascience-notebook /bin/bash
```

 - パスワードをハッシュ文字列化するためのプロンプトを起動します。
```
(dockerコンテナ内部) $ python -c 'from notebook.auth import passwd;print(passwd())'
```
 - 使用したいパスワードの入力を求められるので、2回繰り返し入力します。
  sha1:YOUR_PASSWORD_HASH_VALUE(YOUR_PASSWORD_HASH_VALUEは環境によって異なります。)を、後ほど利用します。

 - Local環境に戻ります。
```
(dockerコンテナ内部) $ exit
```

4. データ分析環境のDockerコンテナを起動する

```
$ docker run  \
    --user root \
    -e GRANT_SUDO=yes \
    -e NB_UID=$UID \
    -e NB_GID=$GID \
    -e TZ=Asia/Tokyo \
    -p 8888:8888 \
    --name notebook \
    -v ~/path/to/directory/:/home/jovyan/work \
    jupyter/datascience-notebook \
    start-notebook.sh \
    --NotebookApp.password='sha1:YOUR_PASSWORD_HASH_VALUE'
```
 - simpele version
```
$ docker run --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v $(pwd):/home/jovyan/work jupyter/datascience-notebook
```

