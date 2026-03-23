# RailsAPIの認証エンドポイントとJWTの流れ

### エンドポイント一覧

| エンドポイント | 役割 | 認証 |
|---|---|---|
| `POST /api/v1/signup` | 新規登録 + token発行 | 不要 |
| `POST /api/v1/login` | 認証 + token発行 | 不要 |
| `GET /api/v1/me` | ログイン中のユーザー情報取得 | 必要 |

---

### token（JWT）の流れ

**サインアップ・ログイン時**
フロント → POST /signup or /login
↓
AuthController が認証・登録処理
↓
encode_token でJWTを生成（ApplicationController に定義）
↓
token + user情報をJSONで返す
↓
フロント ← saveToken() でブラウザに保存
↓
以降のリクエストに token を付けて送る



**認証が必要なAPI呼び出し時**
フロント → Authorization: Bearer <token>
↓
BaseController の authorize_request が実行される
↓
decode_token でtokenを読み込む
↓
current_user をセット
↓
各コントローラーの処理へ



---

### なぜ login/signup でもuser情報を返すのか

tokenだけ返した場合、ログイン直後にユーザー名などを表示するために `/me` を追加で叩く必要がある。
リクエスト数を減らすために、login/signup のレスポンスにuser情報を含めている。今回は、ログイン、サインアップ直後にユーザー情報を出力していないので不要であるが、この設計が一般的である。meは、マイページでユーザー情報のみを取得するためのAPIである。

```json
{
  "token": "xxx.yyy.zzz",
  "user": {
    "id": 1,
    "name": "John",
    "email": "john@example.com"
  }
}
```
