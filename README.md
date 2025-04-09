# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)

## 新しいシステムのセットアップ手順

### フロントエンドのセットアップ

1. **Reactプロジェクトの初期化**
   - `create-react-app`を使用してReactプロジェクトを初期化します。
   ```bash
   npx create-react-app my-app
   cd my-app
   ```

2. **必要なパッケージのインストール**
   - React Routerを使用してルーティングを管理する場合、以下のコマンドを実行します。
   ```bash
   npm install react-router-dom
   ```

3. **基本的なページとコンポーネントの作成**
   - `src/components/Header.js`, `src/components/Footer.js`, `src/pages/Home.js`, `src/pages/About.js`を作成します。

4. **ルーティングの設定**
   - `src/App.js`でReact Routerを使用してルーティングを設定します。

5. **スタイルの追加**
   - `src/index.css`に基本的なスタイルを追加します。

### バックエンドAPIのセットアップ

1. **FastAPIプロジェクトの初期化**
   - Pythonの仮想環境を作成し、FastAPIとUvicornをインストールします。
   ```bash
   python -m venv venv
   source venv/bin/activate  # Windowsの場合は `venv\Scripts\activate`
   pip install fastapi uvicorn
   ```

2. **基本的なAPIの作成**
   - `main.py`を作成し、FastAPIを使用して基本的なAPIエンドポイントを作成します。

3. **サーバーの起動**
   - Uvicornを使用してFastAPIサーバーを起動します。
   ```bash
   uvicorn main:app --reload
   ```

これで、Reactを使用したフロントエンドとFastAPIを使用したバックエンドの基本的なセットアップが完了しました。次のステップとして、データベースのセットアップや認証の実装を進めることができます。

## RPGプログラムのサンプル

以下は、AS400上で動作するRPGプログラムの標準的なロジックを利用したサンプルプログラムです。このプログラムは、顧客情報を読み取り、条件に基づいて処理を行います。

```rpg
     H DFTACTGRP(*NO) BNDDIR('QC2LE') ACTGRP(*NEW)

     F* ファイル仕様
     FCustomer  IF   E           K DISK    Prefix(Cust_)

     D* 変数の定義
     D CustId          S              5P 0
     D CustName        S             20A
     D CustPhone       S             10A
     D CustStatus      S              1A

     C* 顧客情報の読み取りと処理
     C     READ        Customer
     C                   DOW       NOT %EOF(Customer)
     C* 顧客ステータスに基づく処理
     C                   IF        CustStatus = 'A'
     C* アクティブな顧客の処理
     C                   DSPLY                   'Active Customer: ' + CustName
     C                   ELSEIF    CustStatus = 'I'
     C* 非アクティブな顧客の処理
     C                   DSPLY                   'Inactive Customer: ' + CustName
     C                   ELSE
     C* その他のステータスの処理
     C                   DSPLY                   'Unknown Status for: ' + CustName
     C                   ENDIF
     C* 次のレコードを読み取る
     C                   READ      Customer
     C                   ENDDO

     C* プログラムの終了
     C                   SETON                                        LR
```

### プログラムの説明

- **ファイル仕様（F-Spec）**: `Customer`という物理ファイル（テーブル）を定義します。`IF`は入力ファイルを示し、`K`はキー付きファイルを示します。
- **データ仕様（D-Spec）**: 顧客ID、名前、電話番号、ステータスを格納する変数を定義します。
- **計算仕様（C-Spec）**: 顧客情報を読み取り、ステータスに基づいて処理を行います。
  - `READ`命令で顧客レコードを順次読み取ります。
  - `DOW NOT %EOF(Customer)`でファイルの終わりまでループを続けます。
  - `IF`文を使用して、顧客のステータスに基づく処理を行います。
  - `DSPLY`命令で処理結果を表示します。
  - `READ`命令で次のレコードを読み取ります。
  - `SETON LR`でプログラムを終了します。

このサンプルプログラムは、RPGでの標準的なファイル処理と条件分岐のロジックを示しています。

## Docker環境でのセットアップ

### フロントエンドのDockerセットアップ

1. **Dockerfileの作成**

   フロントエンドのプロジェクトディレクトリに`Dockerfile`を作成します。

   ```dockerfile
   # ベースイメージ
   FROM node:14

   # 作業ディレクトリの設定
   WORKDIR /app

   # パッケージファイルをコピーして依存関係をインストール
   COPY package*.json ./
   RUN npm install

   # アプリケーションのソースをコピー
   COPY . .

   # アプリケーションをビルド
   RUN npm run build

   # アプリケーションを起動
   CMD ["npm", "start"]
   ```

### バックエンドのDockerセットアップ

1. **Dockerfileの作成**

   バックエンドのプロジェクトディレクトリに`Dockerfile`を作成します。

   ```dockerfile
   # ベースイメージ
   FROM python:3.9

   # 作業ディレクトリの設定
   WORKDIR /app

   # 依存関係をインストール
   COPY requirements.txt ./
   RUN pip install --no-cache-dir -r requirements.txt

   # アプリケーションのソースをコピー
   COPY . .

   # アプリケーションを起動
   CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
   ```

### Docker Composeファイルの作成

プロジェクトのルートディレクトリに`docker-compose.yml`を作成し、フロントエンドとバックエンドサービスを定義します。

```yaml
version: '3'
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development

  backend:
    build: ./backend
    ports:
      - "8000:8000"
    volumes:
      - ./backend:/app
    environment:
      - DATABASE_URL=postgresql://user:password@db/myapp

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

### 実行手順

1. **Docker Composeを使用してサービスを起動**

   プロジェクトのルートディレクトリで以下のコマンドを実行します。

   ```bash
   docker-compose up --build
   ```

これで、フロントエンドとバックエンドの両方がDockerコンテナで実行されるようになります。PostgreSQLデータベースもコンテナで実行され、バックエンドと連携します。Dockerを使用することで、環境の一貫性を保ちながら、簡単にアプリケーションをデプロイできます。

もし、`docker-compose.yml`ファイルが見つからないというエラーが出ている場合は、ファイルが正しいディレクトリに存在するか確認してください。また、`docker-compose.yml`の内容が正しいかも確認してください。
"# as400" 
