## Following the DHH video at  https://rubyonrails.org/

1. `docker-compose run app rails new . --force --database=mysql` generate all the base rails application files.
2. replaced the `config/database.yml` with `SavedFiles/database.yml
3. `docker-compose run app bin/rails db:create` to create the database
4. `docker-compose build` build our docker image 
5. `docker-compose up -d` start our docker image
6. `docker exec -it cs351Rails bash` enter the docker image 
7. `rails generate scaffold post title:string content:text` create the post resource.
8. Examine the migration, model, controller, and view templates created for `post`
9. `rails db:migrate` to update the database with the new post table 
10. Examine http://localhost:3001 to see the default rails page 
11. Examine http://localhost:3001/posts to see the post pages
12. Edit MyRailsApp/app/views/layouts/application.html.erb
    add `<link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">` to the head to improve the site style 
13. Examine http://localhost:3001/posts.json to see the json version posts 
14. Edit `/app/model/post.rb`, add `validates_presence_of :title` to add validation. 
15. Try to submit a post without a title
16. In docker image open the console with `rails console`
17. In Rails console try:
    1. `Post.first`
    2. `Post.last`
    3. `Post.create! title: "From the console", content: "I love command line!"`
    4. Examine http://localhost:3001/posts
    5. `Post.all`
    6. `Time.now`
    7. `Time.now.all_day`
    8. `Time.now.yesterday`
    9. `Time.now.yesterday.all_day`
    10. `Post.where(created_at: Time.now.all_day)`
    11. `Post.where(created_at: Time.now.yesterday.all_day)`
    12. `exit` to exit to rails console
18. In docker image open the console:
    1. `rails action_text:install` to add action text.
    2. `bundle install` to install the new gems
    3. `rails db:migrate` to update the database with the new tables
19. Now we need to rebuild rails and restart rails.
    1. `exit` to leave the docker image 
    2. `docker-compose down`
    3. `docker-compose build`
    4. `docker-comose up -d`
    5. Edit `/app/views/posts/_form.html.erb` change the line (21) from:
       * `<%= form.text_area :content %>` to read 
       * `<%= form.rich_text_area :content %>`
    6. Edit `/app/model/post.rb` add ` has_rich_text :content` after the validation line.
    7. Create a new post in the browswer with formatted text.  
        * Do not try and upload an image.  This doesn't seem to work with our docker image right now. 
20. Back in docker image shell run 
    1. `rails generate resource comment post:references content:text`    
    2. `rails db:migrate` to update the database with the new comments table
    3. Edit `/app/model/post.rb` add 
    4. `rails console` to open a rails console
       1. `Post.first.comments` to see the comments, there are none 
       2. `Post.first.comments.create! content: "First Comment"` to craate the first comment
       3. `Post.first.comments` to see the comments
       4. `exit` to leave the console
21. Edit `app/views/posts/show.html.erb` add `<%= render "posts/comments", post: @post %>` under `<%= render @post %>`
22. Copy `_comments.html.erb` and `_new.html.erb` out of SavedFiles and into `app/views/comments/`
23. Edit `app/views/posts/_post.html.erb` add `<p><string><%= pluralize post.comments.count, "comments" %></string></p>` around line 6.
24. Copy the `comments_controller.rb` file from SavedFiles and replace `app/controllers/comments_controller.rb`
25. Edit `config/routes.rb` change the two lines:
    ``` 
    resources :posts do
    resources :comments
    ```
    To
    ```
    resources :posts do
       resources :comments
    end
    ```
    In oder to next the comments route in posts.


And that's where we'll stop.  David goes on to add a mailer and other features if you'd like to continue with the demo.  

