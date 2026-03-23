# RailsAPIの認証コントローラー設計

## 概要
Rails API における認証まわりのコントローラー設計メモ。

### ApplicationController が親
ApplicationController
└── BaseController

### BaseController が親
BaseController
├── PostsController
├── ProjectsController
└── AuthController（authorize_request をスキップ）

## 各コントローラーの役割

### ApplicationController
- 全コントローラーの最上位
- encode_token を定義
- 認証済み・未認証どちらからも使えるようにここに置く

### BaseController
- 認証が必要な全エンドポイントの親
- before_action :authorize_request で全リクエストを認証チェック
- decode_token を定義（BaseController 配下でしか使わないため）
- current_user を設定して各コントローラーに渡す

### AuthController
- ログイン・新規登録専用
- skip_before_action で認証チェックをスキップ
- encode_token でJWTを発行してフロントに返す

## JWT の流れ
1. ログイン → encode_token でJWT発行 → localStorage に保存
2. 以降のリクエスト → Authorization ヘッダーにJWTを付けて送信
3. decode_token でJWTを読む → current_user を特定 → アクション実行

## private について
外部から直接呼び出せないようにするアクセス制御。
Railsはpublicメソッドをアクション（URL）として認識するため、
内部処理は private にして外部公開を防ぐ。
