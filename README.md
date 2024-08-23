# OpenAPI Generator Sample

このリポジトリは、OpenAPI仕様を作成・編集し、それを元にAPIコードを自動生成するためのサンプルプロジェクトです。Dockerを使用して、OpenAPIのエコシステムを手軽に試すことができます。

## 必要要件

このプロジェクトを実行するには、以下が必要です:

- **Docker**: コンテナの管理およびサービスの実行に使用します。
- **docker-compose**: 複数のサービスを一括で管理するためのツールです。

## セットアップ

### 1. リポジトリのクローン

まず、このリポジトリをローカル環境にクローンします。

```sh
git clone https://github.com/kaaaaakun/openapi-generator-sample.git
cd openapi-generator-sample
```

### 2. Dockerサービスの起動

次に、Docker Composeを使用して必要なサービスを起動します。以下のコマンドを実行してください。

```sh
docker-compose up -d
```

このコマンドは、バックグラウンドで以下のサービスを立ち上げます。

- **apisprout**: 軽量なAPIモックサーバー
- **swagger-ui**: OpenAPIドキュメントをGUIで表示するツール
- **swagger-editor**: OpenAPIドキュメントを作成・編集するためのエディタ
- **openapi-generator**: OpenAPI仕様からコードを自動生成するツール

### 3. サービスへのアクセス

Dockerコンテナが正常に起動したら、以下のURLにアクセスして各サービスを利用します。

| サービス名       | 説明                                                                                 | URL                        |
|------------------|--------------------------------------------------------------------------------------|----------------------------|
| **apisprout**    | OpenAPI仕様に基づいてモックAPIを提供します。                                          | [localhost:8000](http://localhost:8000) |
| **swagger-ui**   | OpenAPIドキュメントを視覚的に確認できます。                                            | [localhost:8081](http://localhost:8081) |
| **swagger-editor**| OpenAPI仕様を作成・編集できるインターフェースを提供します。                             | [localhost:8082](http://localhost:8082) |

#### Swagger-Editorの使い方

- **アクセス**: ブラウザで [http://localhost:8082](http://localhost:8082) を開きます。
- **エディタ**: OpenAPI仕様のYAMLを左側のエディタに入力します。右側にはリアルタイムでエラーチェックとプレビューが表示されます。
- **エクスポート**: 仕様をエクスポートすることで、実際のAPI仕様書（YAMLファイル）として保存できます。

#### Swagger-UIの使い方

- **アクセス**: [http://localhost:8081](http://localhost:8081) をブラウザで開きます。
- **ドキュメント表示**: `swagger/api.yaml`に記載されたOpenAPIドキュメントがGUI形式で表示されます。各エンドポイントをクリックすると、詳細を確認できます。

#### APIモックサーバー（apisprout）の使用例

以下のコマンドをターミナルで実行して、モックAPIからデータを取得します。

```sh
curl http://localhost:8000/pets
```

**レスポンス例**:
```json
[
  {
    "id": 0,
    "name": "string",
    "tag": "string"
  }
]
```

このモックデータは、`swagger/api.yaml`で定義された仕様に基づいて返されます。

### 4. OpenAPI Generatorでのコード生成

OpenAPI Generatorを使用して、指定した言語のコードを生成することができます。以下のコマンドを使用します。

```sh
docker-compose run --rm openapi-generator generate -i /swagger/api.yaml -g {language} -o /app
```

- **`-i`**: 入力ファイルの指定。`swagger/api.yaml`を指定します。
- **`-g`**: 生成するコードの言語を指定します。例えば、`python`や`java`など。
- **`-o`**: 出力先ディレクトリを指定します。ここでは`/app`が指定されています。

#### 使用例: Pythonコードの生成

```sh
docker-compose run --rm openapi-generator generate -i /swagger/api.yaml -g python -o /app
```

このコマンドは、OpenAPI仕様に基づいてPythonクライアントコードを`/app`ディレクトリに生成します。

### 5. 生成されたコードの確認

生成されたコードは、`api-skeleton/`ディレクトリに保存されます。このディレクトリには、APIクライアントのスケルトンコードが含まれており、実際の開発に利用できます。

## ディレクトリ構造

以下は、プロジェクトのディレクトリ構造の概要です。

```plaintext
.
├── LICENSE
├── README.md
├── api-skeleton
│   └── 生成されたコード
├── docker-compose.yml
├── nginx
│   └── default.conf
└── swagger
    └── api.yaml
```

## 参考リンク

以下のリソースを参照して、OpenAPIおよび関連ツールの詳細を確認できます。

- [apisprout](https://github.com/danielgtaylor/apisprout): 軽量なAPIモックサーバー
- [swagger-ui](https://swagger.io/tools/swagger-ui/): OpenAPIドキュメントのビジュアル化ツール
- [swagger-editor](https://editor.swagger.io/): OpenAPI仕様の作成・編集ツール
- [OpenAPI Generator](https://github.com/OpenAPITools/openapi-generator): OpenAPI仕様からコードを自動生成するツール
