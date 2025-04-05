# Rails Generate Controller Command & Crud Action Details

In Ruby on Rails, the `rails generate controller` command is used to quickly create a new controller along with its associated view files, routes, and helper files.

---

### 🔧 Basic Command Format

```bash
rails generate controller ControllerName action1 action2 action3 ...
```

Or using the shorthand:

```bash
rails g controller ControllerName action1 action2
```

---

### ✅ Example

```bash
rails g controller Posts index show new create edit update destroy
```

This will generate:

- A controller file: `app/controllers/posts_controller.rb`
- View files:
  - `app/views/posts/index.html.erb`
  - `app/views/posts/show.html.erb`
  - `app/views/posts/new.html.erb`
  - `app/views/posts/edit.html.erb`
- A helper: `app/helpers/posts_helper.rb`
- Routes added to `config/routes.rb` (if Rails 6+ and `--skip-routes` is not used)
- Test files (if using default test setup)

---

### 🧱 CRUD Actions Breakdown

| Action   | HTTP Verb | Path             | Purpose                     |
|----------|-----------|------------------|-----------------------------|
| `index`  | GET       | `/posts`         | List all records            |
| `show`   | GET       | `/posts/:id`     | Show a specific record      |
| `new`    | GET       | `/posts/new`     | Render form to create       |
| `create` | POST      | `/posts`         | Submit new record           |
| `edit`   | GET       | `/posts/:id/edit`| Render form to edit         |
| `update` | PATCH/PUT | `/posts/:id`     | Submit record update        |
| `destroy`| DELETE    | `/posts/:id`     | Delete the record           |

---

### 📌 RESTful Resource in `routes.rb`

You can add this manually or let Rails do it when you generate the controller:

```ruby
resources :posts
```

This will generate all 7 RESTful routes.

---

### 📝 Sample Controller (`posts_controller.rb`)

```ruby
class PostsController < ApplicationController
  def index
    @posts = Post.all
  end

  def show
    @post = Post.find(params[:id])
  end

  def new
    @post = Post.new
  end

  def create
    @post = Post.new(post_params)
    if @post.save
      redirect_to @post
    else
      render :new
    end
  end

  def edit
    @post = Post.find(params[:id])
  end

  def update
    @post = Post.find(params[:id])
    if @post.update(post_params)
      redirect_to @post
    else
      render :edit
    end
  end

  def destroy
    @post = Post.find(params[:id])
    @post.destroy
    redirect_to posts_path
  end

  private

  def post_params
    params.require(:post).permit(:title, :body)
  end
end
```

---

Want a custom scaffold template or a version with API-only responses? Let me know!