# Hacker News
## Delivery
### 1.

Link to our backend (REST API):
http://139.59.211.36:8081

Link to our frontend:
http://139.59.211.36:8083

Link to our logging system:
http://139.59.211.36:8089

Currently, all of our subsystems are running on the same DigitalOcean droplet.

## Routes in our REST-API

### Getting posts
#### '/posts/:skip/:limit'
- METHOD: GET
- Skip is an integer that decides how many documents you want to skip before getting x amount of posts
- Limit is an integer that decides how many documents you want from the backend
##### Example
'/posts/25/25' will give you the documents 25-50. We skipped the first 25 documents and wanted to get 25 documents

### Getting post
#### '/post/:id'
- METHOD: GET
- Id is the ID of the document you want to get

You will get the following response. This route will also get all comments for at post. The comments will also have an array of comments, if the comments have comments themselves. 
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
