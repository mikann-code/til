`resources` にカスタムルートを追加するとき、`collection` と `member` の2種類がある。

---

### collection

IDを必要としないルート。リソース全体に対する操作。

```ruby
collection do
  get :archived
end
# → GET /api/v1/projects/archived
```

---

### member

IDを必要とするルート。特定の1件に対する操作。idで特定の1件を取得している。

```ruby
member do
  patch :archive
  patch :unarchive
end
# → PATCH /api/v1/projects/:id/archive
# → PATCH /api/v1/projects/:id/unarchive
```

---

### まとめ

| | URL | 用途 |
|---|---|---|
| `collection` | `/projects/archived` | 全体・一覧系の操作 |
| `member` | `/projects/:id/archive` | 特定1件への操作 |

**判断基準：IDが必要かどうか**
- IDなし → `collection`
- IDあり → `member`
