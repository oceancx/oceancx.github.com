---
---

### 建立一个Rails项目

1. rails new <project name> -d mysql
2. cd <project dir>
3. rails s

如果运行3运行失败

1. 如果是rails 4.2.4 那么在 Gemfile 中 mysql2 后面 要加 '~> 0.3.18'
2. 如果是mysql错误 那么就 rake db:create db:migrate
3. 如果还错，那么就在config/database.yml 中输入密码

### 建立数据表结构

1. 更改数据库的表结构，Rails给出的方法是 [Migration](http://guides.rubyonrails.org/)

	rails g migration CreateIssues

2. 可以在生成的文件中添加需要的字段：

	class CreateIssues < ActiveRecord::Migration
	  def change
	    create_table :issues do |t|
	      t.string :title
	      t.timestamps
	    end
	  end
	end

3. 运行

	rake db:migrate
把修改内容真正写进mysql数据库。会生成 db/schema.rb文件


### 添加新字段到数据库

	rails g migration AddContentToIssues content:text
	rake db:migrate

### 生成controller
	
	rails g controller issues show # 注意是issues 不是 issue

### 对资源的操作

- #### 资源的显示 issues#show

 route: get '/issues/:id'  =>(to:) 'issues#show'
 例如 GET /issues/17  会传到controller的show方法中 同时将{id: '17'}放入到params中

- #### 资源的删除 issues#destroy
	
  route: delete 'issues/:id' => 'issues#detroy'

- #### 资源的创建 issues#new

  route: get 'issues/new' => 'issues#new' # 这一行要写在 get '/issues/:id' 上面 (显示表单  controller里面的是 Issue.new)

 没有 get 'issues' => 'issues#index', :as => 'issues'会报错 'undefined method `issues_path''
 添加 get 'issues' => 'issues#index' , :as => 'issues'

 点击提交按钮,启动路由 post 'issues' => 'issues#create'


- #### 资源的编辑 issues#edit 

  路由: get 'issues/:id/edit' => 'issues#edit', :as => 'edit_issue' 创建编辑表单

  这里可以看出 form_for 是很智能的，如果传递给他的是一个新对象，就行在 issues#new 中，那他就会提交到 POST issues/ 而如果给他的时已经赋值过的对象，那他就会提交到 PATCH issues/，

  patch 'issues/:id' => 'issues#update'

一个资源一共7个路由

1. GET /photos photos#index
2. GET /photos/new photos#new
3. POST /photos photos#create
4. GET /photos/:id photos#show
5. GET /photos/:id/edit photos#edit
6. PATCH/PU /photos/:id photos#update
7. DELETE /photos/:id photos#destroy


### 笨方法发送消息

	+ <%= render partial: 'issue_list', locals: { issues: @issues } %>

partial对应文件名: \_issue\_list.html.erb

### Rails 连接
[Ruby on Rails ② rails常用的验证](http://blog.sina.com.cn/s/blog_63041bb80101dgdu.html)


如果是在production的环境下，应该用rails db进入。（自己推测可能是在开发环境下，对数据库进行了加密）。[相关Link](https://ruby-china.org/topics/6919).