# Design a URL shortener

## 1. Scope the Project

### Features
**So, what are we building? What features might we need?**

a) Is this a full web app with a web interface?
  - No, let's just build an API to start.

b) Since it's an API, do we need authentication or user accounts or developer keys?   
  - No, let's just make it open to start.

c) Can people modify or delete links?
  - Let's leave that out for now.

d) If people can't delete links, do they persist forever? Or do we automatically remove old ones?

  - **Removing old ones**
    - Remove links that were *created* some length of time ago
    - Remove links that haven't been *visited* in some length of time ago


  - Let's let links exist forever.

d) Should we let people choose their shortlink, or just always auto-generate it?
  - Let's allow people to choose.

e) Do we need analytics, so people can see how many people are clicking on a link, etc?
  - Good idea. But let's leave it out to start.

It's okay if your list of features was different from ours. Let's proceed with these requirements so we're working on the same problem.

### Design Goals
**What are we optimizing for?**
