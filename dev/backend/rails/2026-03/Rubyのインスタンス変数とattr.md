# Rubyのインスタンス変数とattr

### インスタンス変数とは
- 頭に @ をつける変数
- 同じクラス内であれば違うメソッドでも参照できる
- クラスの外からは直接参照できない

### ローカル変数との違い
| 種類 | 書き方 | 使える範囲 |
|---|---|---|
| ローカル変数 | name | そのメソッド内だけ |
| インスタンス変数 | @name | 同じクラス内の全メソッド |

### 他クラスからの参照を許可する
attr_reader :current_user
# @current_user を他のクラスから読み取れるようにする

### attr の種類
| | 読み取り | 書き込み |
|---|---|---|
| attr_reader | ○ | × |
| attr_writer | × | ○ |
| attr_accessor | ○ | ○ |

### BaseController での使われ方
- authorize_request で @current_user を設定
- attr_reader により PostsController など子クラスから参照できる
- BaseController で1回定義・設定するだけで、継承している全ての子クラスで current_user が使えるようになります。
