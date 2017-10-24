# Hacker News

## 2 - Logical Data Model

![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Logical%20Data%20Model.png)

## 3 - Use Case Model

### Use Case Diagram

![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Use%20Case%20Diagram.png)

### Use Cases

#### Login

 - **Name:** Login
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To enable restricted content
 - **Primary Actor:** The previously registered user
 - **Precondition:** The user is on the site but not logged in
 - **Main success scenario:** The user successfully logs in
 - **Success Guarantees:** The user is logged in and can view restricted content
 - **Extension:** NONE
 - **Special Reqs.:** NONE

#### Register

 - **Name:** Register
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To gain access to restricted content
 - **Primary Actor:** The guest
 - **Precondition:** The guest is on the site but not registered
 - **Main success scenario:** The guest successfully registers
 - **Success Guarantees:** The user is able to log in
 - **Extension:** NONE
 - **Special Reqs.:** NONE

#### Make Post

 - **Name:** Make Post
 - **Scope:** System under design (SuD)
 - **Level(Goal):** Share content with other users/guests
 - **Primary Actor:** The registered user
 - **Precondition:** The user is logged in and has a great news story to share
 - **Main success scenario:** The user successfully makes a post
 - **Success Guarantees:** The post is created
 - **Extension:** NONE
 - **Special Reqs.:** NONE

#### View Post

 - **Name:** View Post
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To view the content of a post and its comments
 - **Primary Actor:** Guest/User
 - **Precondition:** The person is on the site
 - **Main success scenario:** The user successfully clicks the post
 - **Success Guarantees:** The post is retrieved and viewed
 - **Extension:** NONE
 - **Special Reqs.:** NONE

#### Vote Post

 - **Name:** Vote Post
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To express your like/dislike for a specific post
 - **Primary Actor:** Reg. User
 - **Precondition:** The user is logged in and viewing the post (to negatively vote the user needs 500 karma points)
 - **Main success scenario:** The user successfully votes on the post
 - **Success Guarantees:** The posts rating/points is updated to reflect the vote
 - **Extension:** NONE
 - **Special Reqs.:** NONE

#### Comment Post

 - **Name:** Comment Post
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To express your opinion or discuss the content of the post
 - **Primary Actor:** Reg. User
 - **Precondition:** The user is logged in and viewing the post
 - **Main success scenario:** The user successfully comments
 - **Success Guarantees:** The posts comment list is updated to reflect the submission
 - **Extension:** NONE
 - **Special Reqs.:** NONE

#### Find Post

 - **Name:** Find Post
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To look up a post with a certain keyword, tag etc...
 - **Primary Actor:** Reg. User/Guest
 - **Precondition:** The person is on the site
 - **Main success scenario:** The person recieves a list of posts reflecting the search
 - **Success Guarantees:** The list appears as if out of nowhere, gracefully leading the user to the desired post
 - **Extension:** NONE
 - **Special Reqs.:** NONE
 
 #### Reply Comment

 - **Name:** Reply Comment
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To express your opinion about another users opinion or further discuss a matter
 - **Primary Actor:** Reg. User
 - **Precondition:** The user is viewing the post and comment
 - **Main success scenario:** The user successfully writes and submits a reply
 - **Success Guarantees:** The comment section is updated to reflect the added comment
 - **Extension:** NONE
 - **Special Reqs.:** NONE
 
  #### Vote Comment

 - **Name:** Vote Comment
 - **Scope:** System under design (SuD)
 - **Level(Goal):** To express your like/dislike for a specific comment
 - **Primary Actor:** Reg. User
 - **Precondition:** The user is viewing the post and comment
 - **Main success scenario:** The user successfully votes
 - **Success Guarantees:** The comment section is updated to reflect the vote
 - **Extension:** NONE
 - **Special Reqs.:** NONE

 ## Actors
 
 ### User
 
 A person registered on the site as a user. Drives the content-side of the application by submitting content and up/down -voting and commenting on posts/comments.
 
 ### Guest
 
 A person visitting the site. Views, but doesn't interact with, the content of the site.
 
 ## Sequence Diagram
 
 ![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Sekvens%20Diagram.png)
 
Illustrates the flow of viewing posts: The user retrieves the list of new posts, selects one, and views the specific post and its comments.
