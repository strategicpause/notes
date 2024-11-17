Link: https://aws.amazon.com/builders-library/reliability-and-constant-work/

* Introduction
    * The blog post starts with Colm’s personal take on the Nighthawks painting. It serves as an anchor to the grab the attention of the reader and transition the post to the core topic (constant work).
    * The coffee urns in the painting serves a way to help the reader relate to the idea of constant work. A coffee barista could create each cup of coffee by hand, but this approach does not scale to 100 cups. This will result in long lines of poeple during a busy period. Instead the coffee urns allow a restaurant to serve a few dinners, or those 100 cups.
    * Coffee urns don’t have to do more work because more people want more coffee. Instead they perform O(1) amount of work.
    * The tone of the post is a more conversational, and casual, as if. you’re chatting with an acquaintance or a colleague. It’s more concerned about clearly communicating the message, rather than being precise.
* Computers: They do exactly as you tell them
    * This transitions from talking about the properties of the urns that we like (e.g., repeatable, and not having to trade away quality for scale), to computers.
    * When operations performed get slower, then queues start to form, much like at a coffee shop. However, this can lead to a spiral of doom as retries occur.
    * The post references other posts including how to get timeouts and retries right and https://aws.amazon.com/builders-library/using-load-shedding-to-avoid-overload/ to show that even when you get these other aspects right, you can still build a system which results in the spiral of doom.
    * In order to build a reliable system, you should instead strive for dumb, and reliable constant work patterns. This is where the main point of the article starts.
    * Three properties you should care about with these systems:
        * They do not scale up or slow down with load or stress.
        * They do not have modes, which means they do the same operation under all conditions.
        * If they have any variation, it is do less work in times of stress, so they can perform better when you need them most.
    * Colm mentions the term anti-fragility which is a system that has some of the following properties
        * Improvement under stress: Unlike fragile systems that break under pressure, or robust systems that merely resist change, anti-fragile systems improve when exposed to stressors, volatility, or disorder.
        * Embracing uncertainty: Anti-fragile systems thrive in environments of uncertainty and randomness.
        * Learning from failure: These systems use small failures as opportunities to adapt and improve, making them more resilient over time.
        * Decentralization: Anti-fragile systems often benefit from decentralization, as it allows for localized failures without compromising the entire system.
        * Optionality: Maintaining options and flexibility is crucial for anti-fragility, as it allows for adaptation to changing circumstances.
    * A cache is commonly-thought of as having anti-fragile properties as they tend to help response times under load. However, caches have modes; when there is a cache miss, then it can lead to cascading failures. Caches are covered further in https://aws.amazon.com/builders-library/caching-challenges-and-strategies/.
* Amazon Route 53 Health Check & Healthiness
	* This section first describes the purpose of a health check, and how they are integrated into existing products like Route53 & ELB load balancers.
	* The article then goes onto show how health checking performs constant work to connect it to the theme of the article. When a host is healthy or unhealthy (influenced by customer rules), the fleet that performs health checks are still performing constant work.
	* One of the properties of the health check fleet is usage of cellular architecture. The team knows the limits for how many health checks each cell can sustain, and can scale by adding additional cells.
	* The key trick used by Route53 is to have health checks send a set of results to aggregators that is sized to the maximum. In the example used, if there are only 10 health checks configured, it will sitll send out a set of 10,000 results where the other 9,990 entries are dummies. This ensures the work that the aggregators are doing won't increase as more health checks are introduced.
	* To make sure DNS updates are made using constant work, the health check aggregators will push a fixed-size table of health-check status to each DNS server. Each query a DNS server handles will always cross-check the health-check table. Even if the first answer is healthy & usable, then the server will still lookup potential answers anyway to make sure the DNS server is always performing the same amount of work.
* S3 As a Configuration Loop
	* AWS Hyperplane will fetch configuration updates from Amazon S3 once every few seconds. It will then process the update to incorporate customer changes such as adding new instances or containers to a load balancer.
* Constant Work & Self-Healing
	* Examples include fixing issues on a next pass (ie: receiving corrections on the next update). These kind of system are constantly starting from a clean slate and always operating in a "repeair everything" mode.
	* Workflows on the other hand need complex logic to handle cases where actiosn do not succeed or need to be repaired due to a transient corruption. 
	* They are also prone to build-up of backlogs.
* Design & Manageability
	* The notion of O(1) means that there are a fixed number of operations being performed regardless of size of the input.
* The value of a simple design
	* A simple design is one that is easy to understand, use, and operate. It should make sense to people outside of the team that wrote and built the system.
	* Summarized as "apply a full configuration each time in a loop."
