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

a) We should be able to store a lot of links, since we're not automatically expiring them.

b) Our shortlinks should be as short as possible. The whole point of a link shortener is to make short links! Having shorter links than our competition could be a business advantage.

c) Following a shortlink should be fast. The shortlink follower should be resilient to load spikes.

**Think about tradeoffs. Which goal is more important?**

### Data Model
**Think about the database schema or the models we'll want.**
  - What things do we need to store, and how should they relate to each other?
  - Is this a many-to-many or a one-to-many?
  - Should these be in the same table or different tables?

**We care about using descriptive and consistent names!**

Let's call our main entity a Link. A Link is a mapping between a shortLink on our site, and a longLink, where we redirect people when they visit the shortLink.


    Link
    - shortLink
    - longLink


The shortLink could be one we've randomly generated, or one a user chose.

Of course, we don't need to store the full ShortLink URL (bit.ly/home), we just need to store the "slug"—the part at the end (e.g. "home").

So let's rename the shortLink field to "slug."


    Link
    - slug
    - longLink


Now the name longLink doesn't make as much sense without shortLink. So let's change it to destination.


    Link
    - slug
    - destination

And let's call this whole model/table ShortLink, to be a bit more specific.


    ShortLink
    - slug
    - destination


Investing time in carefully naming things from the beginning is always impressive in an interview. A big part of code readability is how well things are named!

### Views / Pages / Endpoints
**Naive first design**
  - Our main goal here is to come up with a skeleton to start building things out from.
  - Think about what endpoints/views we'll need, and what each one will have to do.

**API - REST-style**

1) Create a ShortLink
  - Endpoint at bit.ly/api/v1/shortlink

  To create a new ShortLink, we'll send a POST request with destination as required and slug as optional.


      $ curl --data '{ "destination": "raymondlee.io"}' https://bit.ly/api/v1/shortlink
    {
      "slug": "o2prKj",
      "destination": "raymondlee.io"
    }


  For now, we'll just reject non-POST requests with an error 501 ("not implemented") for now.

  So our endpoint might look something like this (pseudocode):

      public Response shortlink(Request request) {
        if (request.method() != HttpMethod.POST) {
            return new Response(HttpStatus.ERROR501); // HTTP 501 NOT IMPLEMENTED response
        }

        String destination = request.getData().getDestination();
        String slug = request.getData().getSlug();

        // if the request did not include a slug, make a new one
        if (slug == null) {
            slug = generateRandomSlug();
        }

        DB.insertLink(slug, destination);

        String responseBody = "{ 'slug' : '" + slug + "' }";

        return new Response(HttpStatus.OK200, responseBody); // HTTP 200 OK response
    }

  **Questions to consider**
  - What characters can we use in randomly generated slugs?
  - What characters are allowed in URLs?
  - How do we ensure a randomly generated slug hasn't already been used?
  - If there is such a collision, how do we handle it?


  2) Make a way to follow a ShortLink.

      public Response redirect(Request request) {
        String destination = DB.getLinkDestination(request.getPath());
        return new Response(HttpStatus.FOUND302, destination); // HTTP 302 REDIRECT response
    }

### Slug Generation
**How long should they be, what characters should we allow, and how should we handle random slug collisions?**

- 1) What characters can we allow in our randomly-generated slugs?
  - A - Z, a - z, 0 - 9, special characters: $ - _ . + ! * , ( )
  - take out all of the special characters for readability and typeability unless user is specifying her own slug
  => 26 + 26 + 10 = 62 possible characters

**Mention potential ambiguities and applying rules for l vs 1 and so on but also mention that this is beyond the scope at the moment.**

- 2) How many distinct slugs do we need?
  - About how many slugs do we need to be able to accommodate? This is a good question to ask your interviewer. (100 per minute?)

- 3) how do we generate a random slug?

```   
  public String generateRandomSlug() {
    Random random = new Random();
    final String alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
    final int numChars = 7;
    String result = "";

    for (int i = 0; i < numChars; i++) {
        int randomIndex = random.nextInt(alphabet.length() - 1);
        result += alphabet.charAt(randomIndex);
    }

    return result;
}
```

  **But how do we ensure slugs are unique? Two general strategies:**

  1) "Re-roll" when we hit an already-used slug

  2) Adjust our slug generation strategy to only ever give us un-claimed slugs.
    => **use base conversion**

### Database
**First bottleneck is database read. What kind of DB should we use?**

- Looking at our app, it seems like relational queries aren't likely to be a big part of our app's functionality, even if we added a few of the obvious next features we might want. So let's go with NoSQL for this.

  1) **Indexing**: In a NoSQL context, that means carefully designing our keys. In this case, make the key for each row in the ShortLink table be the slug.

  2) **Memory**: We could put as much of the data in memory as possible, to avoid disc seeks. => in-memory cache system

  > If we did add a caching layer, there are a few things we could talk about:
    - **The eviction strategy**. If the cache is full, what do we remove to make space? The most common answer is an LRU ("least recently used") strategy.
    - **Sharding strategy**. Sharding our cache lets us store more stuff in memory, because we can use more machines. But how do we decide which things go on which shard? The common answer is a "hash and mod strategy"—hash the key, mod the result by the number of shards, and you get a shard number to send your request to. But then how do you add or remove a shard without causing an unmanageable spike in cache misses?
    - **Database sharding**. We could shard our underlying database instead of, or in addition to caching. If the database has a built-in in-memory cache, sharding the data would allow us to keep more of our data in working memory without an additional caching layer! Database sharding has some of the same challenges as cache sharding. Adding and removing shards can be painful, as can migrating the schema without site downtime. That said, some NoSQL databases have great sharding systems built right in, like Cassandra.

  3) **Processing Web Requests**: We should set up multiple webserver workers. We can put them all behind a load balancer that distributes incoming requests across the workers. Having multiple web servers adds some complexity to our database (and caching layers) that we'll need to consider. They'll need to handle more simultaneous connections, for example. Most databases are pretty good at this by default.

  This should get our database reads nice and fast!
