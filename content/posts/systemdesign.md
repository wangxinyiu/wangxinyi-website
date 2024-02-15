---
title: 'System Design'
date: 2024-02-11T16:25:07-08:00
type: post
draft: true
---

- [Design Twitter](#design-twitter)
# Design Twitter
Requirment
1. creating a tweet
2. Viewing (or generating the timeline)
3. Follow people & interact with tweets

Non-functional requirments
1. Oprimize for reads
2. Optimize for influencers
3. high availibility
4. Low lentancy

API  
```
POST:/create/tweet     
body: {         
    text: ""            
}

GET:/getTweets

POST: /follow
body: {
    follow_id: ""
}

POST: /tweet/like
POST: /tweet/:id/reply
body: {
    text: "",
}
```           

Design
{{< mermaid >}}
graph TB
A(Users) -->|POST| B(Load Balancers)
B --> C(API Servers
        /tweets/*)
C --> D(database:
        Users & Tweets)
C --> E(Timeline Generation
        Service)
E --> F(Database: Timeline generation)

G(Users) --> |GET/getTweets|B
B --> E
E --> B
Influencers_service --> E
{{< /mermaid >}}

Timeline Generation Service: Timeline Generation Service is a system that uses algorithms to curate and organize a stream of social media content for users.
