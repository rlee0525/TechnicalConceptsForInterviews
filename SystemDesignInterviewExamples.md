# System Design Interview Examples

> Source: http://blog.gainlo.co/index.php/2016/02/17/system-design-interview-question-how-to-design-twitter-part-1/

## Design Twitter

#### Define the problem
- Compress Twitter to its MVP
  - Follow each other and view others' feeds

#### High-level solution
- Divide the whole system into several core components
  - Data modeling
    - Using RD: user object and feed object. User can follow each other, each feed has a user owner.

  - How to serve feeds
    - fetch feeds from all the people you follow and render them by time

#### Detail questions
- When users followed a lot of people, fetching and rendering all their feeds can be costly. How to improve this?
  - Infinite scroll feature to fetch the most recent N feeds instead of all of them.
  - Caching results

- How to detect fake users?
  - Use ML algorithms to identify related features such as registration date, # of followers, # of feeds, etc to detect if a user is fake.

- Can we order feed by other algorithms? (e.g. based on users' interests)
  - Questions

    1) How to measure the algorithm?
      - Maybe by the average time users spend on Twitter or users interaction like favorite/retweet.

    2) What signals to use to evaluate how likely the user will like the feed?
      - Users relation with the author, # of replies/retweets of this feed, # of followers of the author

- How to implement the @ feature and retweet feature?
  - @ feature: store a list of user IDs inside each feed. When rendering your feeds, you should also include feeds that have your ID in its @ list.
  - retweet feature: inside each feed, a feed ID is stored, which indicates the original post if there's any.

#### Trending topics
- Questions

  1) How to get trending topic candidates?
    - Get most frequent hashtags over the last N hours
    - Get the hottest search queries
    - Fetch the recent most popular feeds and extract some common words or phrases

  2) How to rank those candidates?
    - Rank based on frequency
    - Integrate signals like reply/retweet/favorite numbers/freshness
    - Personalized signals such as # of follows/followers

#### Other features
 - Who to follow
 - Moments
 - Search

## Design Instagram
