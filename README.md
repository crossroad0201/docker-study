

## Dockerイメージの作成

### docker-compose.yamlの作成

* http://qiita.com/ootaken/items/26d37cb34d3b575277e0
* https://github.com/blacklabelops/jenkins/blob/master/docker-compose.yml

### ローカルで動作確認

```sh
$ docker-compose up -d
```

## ECR(ECSリポジトリ)へのDockerイメージの登録

* 以下のメッセージは無視
```text
time="2017-04-22T15:04:42+09:00" level=info msg="Unable to use system certificate pool: crypto/x509: system root pool is not available on Windows"
```

### ECRの作成

https://console.aws.amazon.com/ecs/home?region=us-east-1#/repositories

### Dockerイメージのビルド

```sh
$ docker-compose build
jenkins uses an image, skipping

$ docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
blacklabelops/jenkins   latest              145451325e4f        28 hours ago        269 MB
```

### ECRにログイン

```sh
$ aws ecr get-login --region us-east-1

$ docker login -u AWS -p ****
Login Succeeded
```

### Dockerイメージのタグ付け

```sh
$ docker tag blacklabelops/jenkins:latest ****.amazonaws.com/jenkins:latest
```

### DockerイメージをECRに登録

```sh
$ docker push ****.amazonaws.com/jenkins:latest
The push refers to a repository [****.amazonaws.com/jenkins]
5b9e934e397f: Pushed
  :
23b9c7b43573: Pushed
latest: digest: sha256:6ab28abb52a06aec78fa69e5b72ad53669537c5216515af37645ad********** size: 2622
```

## ECSサービスの開始

### ECSタスク定義の作成

https://console.aws.amazon.com/ecs/home?region=us-east-1#/taskDefinitions

### ECSクラスタの作成

https://console.aws.amazon.com/ecs/home?region=us-east-1#/clusters

* あとで起動したDockerインスタンスにSSHできるように、キーペアを指定（必要に応じて作成）。

### ECSサービスの作成

https://console.aws.amazon.com/ecs/home?region=us-east-1#/clusters > クラスタを選択 > サービス > 作成

