# 為你自己學 Ruby on Rails
## [Reference](https://railsbook.tw/)

## Ruby 生態圈
### Rails 設計哲學
* Convention over Configuration (CoC)
* Don't Repeat Yourself (DRY)

## 環境設定
### Mac 作業系統安裝
```bash
brew install ruby
```

### RVM 管理 Ruby 開發版本
#### 安裝說明
[RVM 官網安裝教學](https://rvm.io/)

#### 使用 RVM
```bash
rvm list known # 列出所有版本的 Ruby
rvm get master # 更新已支援列表
rvm install 2.5.1 # 安裝 Ruby 2.5.1 版
rvm list # 列出已安裝的 Ruby 版本
rvm use 2.5.1 # 使用 Ruby 2.5.1 版為預設版本
rvm system # 回復到系統預設 Ruby 版本
```

### 檢查 Ruby 版本與位置
```bash
ruby -v # 顯示版本
which ruby # 顯示 Ruby 執行檔位置
echo $PATH # 顯示終端指令找尋路徑
```

### Rails 安裝
```bash
gem install rails
rails -v
gem install rails -v 5.1.1 # 安裝不同版本的 Rails
```

### Rails 專案建立
```bash
rails new hello # 建立 hello 專案
cd hello
bin/rails server
```
打開瀏覽器輸入 `http://localhost:3000` 看到 Rails 歡迎畫面
![Welcome Rails](./images/localhost_3000_.png)

## 常用命令
| 指令 | 說明 |
| --- | --- |
| cd | 切換目錄 |
| pwd | 顯示目前位置 |
| ls | 列出檔案目錄列表 |
| mkdir | 建立目錄 |
| touch | 建立檔案 |
| cp | 複製檔案 |
| mv | 移動檔案 |
| rm | 移除檔案 |
| sudo | 暫時取得權限 |

## 第一個 Rails 應用：Blog
1. 新增使用者 (User)
2. 使用者 (User) 可以新增新改刪除文章 (Post)

### 使用者 (User) 功能
| 欄位名稱 | 資料型態 | 說明 |
| :--- | :---: | :--- |
| name | (字串) string | 使用者姓名 |
| email | (字串) string | 使用者郵件 |
| tel | (字串) string | 使用者電話 |

```bash
bin/rails generate scaffold User name:string email:string tel:string
bin/rails g scaffold User name email tel # 上列命令簡寫
bin/rails db:migrate # 執行資料庫遷移檔
```
可以至 http://localhost:3000/users 新增使用者

### 文章 (Post) 功能
| 欄位名稱 | 資料型態 | 說明 |
| :--- | :---: | :--- |
| title | (字串) string | 文章標題 |
| content | (文字) text | 文章內容 |
| is_avaible | (布林值) boolean | 是否上線 |
| user_id | (整數) integer | 使用者編號 |

```bash
bin/rails g scaffold Post title content:text is_avaible:boolean user:references
bin/rails db:migrate # 執行資料庫遷移檔
```

可以至 http://localhost:3000/posts 新增文章
調整使用者顯示 `app/views/posts/index.html.erb` 修改
```ruby
...
<td><%= post.user.name %></td>
...
```

調整 N+1 Query
```ruby
def index
  @posts = Post.all
end
```
改成
```ruby
def index
  @posts = Post.includes(:user)
end
```

### Rails 常用快速鍵
| 原本的指令 | 簡寫 | 用途 |
| :--- | :--- | :--- |
| bin/rails generate | bin/rails g | 用來產生各種需要的檔案，例如 scaffold、controller、model 等等
| bin/rails destroy | bin/rails d | 可刪除產生器所產生的檔案
| bin/rails server | bin/rails s | 啟動 Rails 伺服器，讓你可以檢視目前專案的成果
| bin/rails console | bin/rails c | 類似 Ruby 的 IRB 介面，但是有載入整個 Rails 專案的環境，可以在這裡直接操作資料
| bin/rails dbconsole | bin/rails db | 直接進到資料庫裡，使用 SQL 語法對資料庫進行存取
| bundle install | bundle | 安裝套件
| rake test | rake | 執行測試
