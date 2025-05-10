# project-group8
**BlinkFeed Project Proposal**
Contributors: Shaaz Charania, Sophia Ylinen, Jia Wu, Taise Nish

**Project Description**
Our application, BlinkFeed, helps university students and young professionals seeking a relaxed, low-stakes environment to share their thoughts, updates, and ideas. As opposed to traditional social media spaces like X or Threads, populated by posts that will live forever and follow the user on the internet, BlinkFeed introduces ephemeral microblogging. Every post that a user makes will expire in 24 hours, permitting them the ability to post and share without having to worry about creating a digital footprint that will persist forever. Whether it is a discussion about the latest activities on campus, the ever-changing wanderings of the user's mind, or even just a temporary post in relation to a study group, our users love the ability to share something in-the-moment with the knowledge that it will disappear. 
Our audience has actively expressed a desire for platforms that embrace impermanence and spontaneity, which is often missing from mainstream social media platforms. Posts created for these platforms often feel performed, labelled or shared descriptively to avoid long term memories. By implementing an automatic expiration to user posts, BlinkFeed provides a no-pressure situation where the social media "curated version" of a user doesn't need to be perfect; they can share however and whenever they want to. Users are able to utilize BlinkFeed to provide candid or raw posts with the comfort of knowing that their posts will disappear after their preferred time frame. As such, BlinkFeed begins to emulate a more stress-free online experience that can also infuse creativity and real-time sharing experiences.
As developers, we’re excited to build this application because it blends technical challenges with a meaningful user experience. Not only will it allow us to apply what we’ve learned about user authentication, data management, and azure deployment, but it also gives us the chance to explore dynamic backend features like scheduled content deletion. More importantly, we believe that building a microblogging platform focused on ephemerality responds to a growing desire for more mindful, less permanent social interactions online. BlinkFeed isn’t just a technical exercise for us—it’s a way to rethink how digital spaces can support authentic communication.


**Technical Description**

**[Architecture Diagram] (https://miro.com/app/board/uXjVI5R1ZAQ=/)**



| Priority | Role            | User Story                                   | Technical Implementation                                              |
| -------- | --------------- | -------------------------------------------- | --------------------------------------------------------------------- |
| P0       | Visitor         | I want to register/login.                    | POST `/register` or `/login` → Azure Auth / Sessions                  |
| P0       | Registered User | I want to create a microblog.                | POST `/posts` → Store in MongoDB with TTL index                       |
| P0       | Registered User | I want to view recent posts (within 24 hrs). | GET `/posts` → Query sorted posts from MongoDB                        |
| P0       | Registered User | I want to delete my own post.                | DELETE `/posts/:postId` → Auth + ownership check                      |
| P1       | Registered User | I want to upvote/downvote posts.             | POST `/posts/:postId/reactions` → Update vote count in MongoDB        |
| P1       | Registered User | I want to view my profile and posts.         | GET `/users/:username` → Return user data + their posts               |
| P1       | Registered User | I want to comment on posts.                  | POST `/posts/:postId/comments` → Append comment with author/timestamp |
| P1       | Registered User | I want to view all comments on a post.       | GET `/posts/:postId/comments`                                         |
| P1       | Visitor         | I want to see trending hashtags.             | GET `/trending` → MongoDB aggregation of hashtags                     |


**Endpoints**
| Method | Endpoint                  | Purpose                                          |
| ------ | ------------------------- | ------------------------------------------------ |
| POST   | `/register`               | Register a new user account                      |
| POST   | `/login`                  | Authenticate user and start session or issue JWT |
| POST   | `/posts`                  | Create a new microblog post                      |
| GET    | `/posts`                  | Get all recent (non-expired) posts               |
| DELETE | `/posts/:postId`          | Delete a post (owner only)                       |
| POST   | `/posts/:postId/upvote`   | Upvote a post                                    |
| POST   | `/posts/:postId/downvote` | Downvote a post                                  |
| POST   | `/posts/:postId/comments` | Add a comment to a post                          |
| GET    | `/posts/:postId/comments` | View all comments for a post                     |
| GET    | `/users/:username`        | View user profile and their posts                |
| GET    | `/trending`               | Get trending hashtags                            |


## Appendix: Database Schemas

### User
```js
{
  _id: ObjectId,
  username: String,
  email: String,
  createdAt: Date
}

Post
{
  _id: ObjectId,
  author: ObjectId,
  content: String,
  createdAt: { type: Date, default: Date.now },
  upvotes: { type: Number, default: 0 },
  downvotes: { type: Number, default: 0 },
  comments: [
    {
      author: ObjectId,
      text: String,
      createdAt: Date
    }
  ],
  hashtags: [String]
}
