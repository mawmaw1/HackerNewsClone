# Hacker News
## Delivery
### 1.

Link to our backend (REST API):
http://139.59.211.36:8081

*This is the server you must use to post posts/comments and get the latest hanesst_id*.

Link to our frontend:
http://139.59.211.36:8083

Link to our logging system:
http://139.59.211.36:8089

Currently, all of our subsystems are running on the same DigitalOcean droplet.

## Submitting issues

Please use the GitHub-issue system when you have detected an error or if the SLA metrics are not living up to the given percentages/uptime etc.
Please try to be as specific as possible when submitting issues. Also remember to label the issue with a correspondig label to the problem. When submitting errors it would also be nice if you explain what you did leading up to the error.

### Errors
If the system breaks, if the system is down or if the system is responding with erros please use the label **System** when submitting your issue.

### Metrics not living up to the SLA
If the system is not living up to the SLA please use the label **SLA** when submitting your issue.

## Routes in our REST-API

### Posting post to db
#### '/post'
- METHOD: POST
- Body of the request MUST have det following format:

##### Example
```js
post = {
    username: String,
    post_type: String,
    pwd_hash: String, 
    post_title: String,
    post_url: String,
    post_parent: Number,
    hanesst_id: {type: Number, unique: true},
    post_text: String,
}
```

### Getting posts
#### '/posts/:skip/:limit'
- METHOD: GET
- Skip is an integer that decides how many documents you want to skip before getting x amount of posts
- Limit is an integer that decides how many documents you want from the backend
##### Example
'/posts/25/25' will give you the documents 25-50. We skipped the first 25 documents and wanted to get 25 documents.

### Getting post
#### '/post/:id'
- METHOD: GET
- Id is the ID of the document you want to get

You will get the following response. This route will also get all comments for at post. The comments will also have an array of comments, if the comments have comments themselves. 
##### Example
```json
{
  "post": {
    "_id": "59f9fa0e36ec3746596fec65",
    "username": "pg",
    "post_type": "story",
    "pwd_hash": "Y89KIJ3frM",
    "post_title": "Y Combinator",
    "post_url": "http://ycombinator.com",
    "post_parent": -1,
    "hanesst_id": 1,
    "post_text": "",
    "created_at": "2017-11-01T16:45:02.349Z",
    "points": 1,
    "__v": 0
  },
  "comments": [
    {
      "_id": "59f9fa0e36ec3746596fec73",
      "username": "sama",
      "post_type": "comment",
      "pwd_hash": "UJHEFZtpzO",
      "post_title": "",
      "post_url": "",
      "post_parent": 1,
      "hanesst_id": 15,
      "post_text": "&#34;the rising star of venture capital&#34; -unknown VC eating lunch on SHR",
      "created_at": "2017-11-01T16:45:02.955Z",
      "points": 1,
      "__v": 0,
      "comments": [
        {
          "_id": "59f9fa0f36ec3746596fec75",
          "username": "pg",
          "post_type": "comment",
          "pwd_hash": "Y89KIJ3frM",
          "post_title": "",
          "post_url": "",
          "post_parent": 15,
          "hanesst_id": 17,
          "post_text": "Is there anywhere to eat on Sandhill Road?",
          "created_at": "2017-11-01T16:45:03.037Z",
          "points": 1,
          "__v": 0,
          "comments": 0
        }
      ]
    }
  ]
}
```

### Getting comments for post or comment
#### '/comments/:id'
- METHOD: GET
- Id is the **hanesst_id** of the post/comment you want to get comments for.

##### Example
In this example we wanted all comments for the post with **hanesst_id = 1**. We got a docuemnt with single comment. The comment also has a comment itself.

```json
[
  {
    "_id": "59f9fa0e36ec3746596fec73",
    "username": "sama",
    "post_type": "comment",
    "pwd_hash": "UJHEFZtpzO",
    "post_title": "",
    "post_url": "",
    "post_parent": 1,
    "hanesst_id": 15,
    "post_text": "&#34;the rising star of venture capital&#34; -unknown VC eating lunch on SHR",
    "created_at": "2017-11-01T16:45:02.955Z",
    "points": 1,
    "__v": 0,
    "comments": [
      {
        "_id": "59f9fa0f36ec3746596fec75",
        "username": "pg",
        "post_type": "comment",
        "pwd_hash": "Y89KIJ3frM",
        "post_title": "",
        "post_url": "",
        "post_parent": 15,
        "hanesst_id": 17,
        "post_text": "Is there anywhere to eat on Sandhill Road?",
        "created_at": "2017-11-01T16:45:03.037Z",
        "points": 1,
        "__v": 0,
        "comments": 0
      }
    ]
  }
]
```
