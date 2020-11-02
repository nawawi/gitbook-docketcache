---
description: Core Caching Concepts in WordPress.
---

# Caching In WordPress

The purpose of this article is to provide a framework for thinking about caching. It discusses core caching concepts that can be difficult to grasp when first working with caching in web development projects. These concepts are then specifically applied to the WordPress context. Finally, some general tips are given about caching.

## Cache Types

To begin, this section will cover four different types of caches that one may encounter. An understanding of these concepts helps to navigate some of the jargon used when reading about caching systems.

### 1. Run-time Cache

A run time cache is a cache that only lasts the duration of a request. Objects are stored in memory but expelled as soon as the request is completed. Any time that you set a value or the results of a routine to variable and use it multiple times, you are making use of a run-time cache. If you need the same data twice in one request, there is no point in regenerating the data multiple times.

As a WordPress example, the main query and the current post object are stored in the `$wp_query` and `$post` global variables, respectively. When data about the current post is needed, MySQL isn’t queried again; rather, the data is pulled from the `$post` global variable. The run-time cache is a simple and efficient strategy for caching data.

The major problem with the run-time cache is that it only lasts the duration of the request. As soon as the request is completed, the cache is dumped. Even though a visitor would generate data that might be used across multiple requests, that data does not persist across requests and will need to be regenerated for every single request. This problem can be solved with object caching.

### 2. Object Caching

Object caching is the act of moving data from a place of expensive and slow retrieval to a place of cheap and fast retrieval. An object cache is also typically persistent, meaning that data cached during one request is available during subsequent requests.

In addition to making data access much easier, cached data should always be replaceable and regenerable. If an application experiences database corruption \(e.g., MySQL, Postgres, Couchbase\), there will and should be severe consequences for this database \(and let us hope that there is a good backup plan in place\). In contrast with the main data store for the application, if a cache is corrupted, the application should continue to function as the cached data should regenerate itself. No data will be lost, although there will likely be some performance problems as the cache regenerates.

The storage engine for an object cache can be a number of technologies. Popular object caching engines include Memcached, Redis, OPcache and the file system. The caching engine used should be dictated by the needs of the application. Each has its advantages and disadvantages. At a bare minimum, the engine used should make accessing the data more performant than regenerating the data.

The object cache tends to be very critical for the application because it can be used to implement the other caches that will be discussed in this article. In other words, if your object cache is implemented incorrectly, you may undermine the rest of your caching architecture.

### 3. Page Cache

A page cache stores HTML data that represents a single page. In many cases, the page uses the object cache to store its data. In such cases, a page cache is simply a special type of object cache. That said, the page cache can use an entirely different storage engine than the object cache. In fact, two popular choices for a page cache are Varnish and Nginx, which are a reverse proxy implementation of a page cache that stores data separately from the object cache.

Unlike the object cache engine that could be used for the page cache storage, the reverse proxy caching would not be good candidates for object caching storage engines and there are also some major technical limitations for using a reverse proxy cache for an object cache.

It is important to distinguish object and page caches. Page caches can lead to significant performance boosts for a web site with a minimal amount of effort; however, they are limited in that many page caching systems make the assumption that every page is rendered identically for every visitor. In other words, the assumption is often made that the page is never unique for an individual user. If your site meets this requirement, you will experience significant gains from implementing a page cache.

Page caching becomes extremely tricky and nearly impossible when the need for unique page views is introduced. In the case of unique pages for every visitor, effective use of object caching is crucial, but you likely will not see the same gains from only object caching that you will see from the only page cache.

It is always important to remember that, with some exceptions, when you implement a page cache, every user will see the same page. If you develop your site with data that is rendered uniquely for an individual \(e.g., printing the user’s name in the header\), that data will be cached for all users. There are certainly ways around this \(e.g., do not cache logged in views\) and you must consider this when implementing a page cache.

### 4. Fragment Caching

Fragment caching is the act of caching only part of a full page. Fragments are merely objects that are not full pages. It can be really tough to distinguish objects from fragments. When people talk about fragments, they are usually referring to identifiable chunks of a page. For example, a profile widget, footer, or related posts listing would all be considered fragments \(but, ugh, they are also objects\).

Typically, the fragment cache uses the object cache as the storage engine for the fragments. In that sense, a fragment cache is usually nothing more than an object cache that is storing named parts of a page.

## A Caching Metaphor

As the primary purpose of this article is to make sense of the caching concepts presented above, a metaphor will use to enhance the understanding of these concepts with particular emphasis on the page and object caching. Previously stated that we all have experience with caching and the concepts were presented. Let go too deep explanation.

Caching is like buying and storing groceries. When you go to the store, you purchase a variety of items. After returning from the store, you store items in your cabinets, refrigerator and counter. Your tip to the store is an act of caching. Obtaining food from one location and storing it in a new location that allows for cheaper and faster access follows the same principle. Let us compare a specific food item to caching.

Many of us buy eggs for use in different meals. One strategy for purchasing eggs would be to go to a market that sells them individually. If you wake up in the morning and want a 2 egg omelette, you could walk to the market, buy 2 eggs, return home and make the omelette. If you want an omelette for the next’s morning breakfast, you can repeat the process. Most of you will see this as a rather absurd process and will instantly see a more efficient strategy of buying a dozen eggs during a single trip to the market, then storing them in the refrigerator for quick access when making your morning meals.

The process of buying eggs and storing them in your refrigerator is similar to caching objects in web development. The process is similar in that you are moving a resource from a place of difficult access to a place of easy access.

To further improve on this metaphor, one purpose of going to the store is to obtain ingredients to make a meal. A meal is composed of numerous ingredients. If you have the ingredients in your refrigerator or cupboards, you can access those ingredients to compose the meal. The meal, in this case, is analogous to the page cache. The page cache is composed of many components, some of which are cached items. With making a meal, you pull items from your refrigerator or cabinets and you must visit the store if you are missing some items. The meal is then composed of items found in your house and the store. If you are really efficient, then the meal is composed entirely of items found in your house. This is similar to page caching in that if your application takes advantage of an object cache, your page cache can be composed entirely of cached objects that are pulled together to form a single page view.

Sometimes, we can also be really efficient and make extras when we cook a meal. Perhaps when cooking your omelette, you decide to cook 5 omelettes, which you store for later meals. This is similar to a page cache in that you will build the page cache and store that as an object for later use. Rather than making omelettes 5 times, you can make 5 omelettes at once and store them in the fridge for later meals.

But the comparisons are not done there. An object cache can be implemented with various storage engines just as you can store your food in various storage devices. You can put your haul in the refrigerator, the freezer, a cabinet, the counter, on shelves, in the pantry, in the basement, etc. You make these decisions based on what storage options you have, how full the storage devices are, your access to additional storage devices, etc. When deciding on a storage engine for your application, you mull over these same decisions. Just like it makes good sense to put your eggs in the refrigerator instead of the cupboard, it might make more sense to hold your page cache in Varnish vs. Memcached; however, sometimes you do not have a refrigerator at your disposal, you have to improvise and store your eggs in a cooler full of ice.

### Use the Metaphor

The purpose of this metaphor is to make it clear that you know more about caching than you think you do. You have used caching strategies before. You can get a lot of mileage out of comparing caching in web development with the process of retrieving groceries to prepare meals.

For instance, you would laugh at someone who went to the store and bought a cup of flour every time the individual needed a single cup of flour for a meal. You would instantly realize that this strategy is time consuming, inefficient and expensive. As a web developer, you should have the exact same reaction when a developer pings Twitter’s API to get Tweets every time a page is loaded. This is an expensive process that is slow and inefficient. In both situations, you should recognize the importance of getting the objects that you need and storing them in a place that is easier to access for future use.

Remember that object caching is like grocery shopping. Storing the acquired objects is like putting away the groceries in your house. Building a page cache is making the meal from your cached items. By thinking of your application as an analogy for making a meal, you can gain some insight into inefficiencies in your caching strategy.

## Applying Caching Concepts to WordPress

Now that we have a good understanding of core caching concepts, it is time to apply them to WordPress. In this section of the article, we will focus on the object cache and page cache as there are clear correlates to those concepts in WordPress. There is no fragment cache in WordPress, so that will not be discussed.

### 1. WordPress Object Caching

WordPress implements an object cache through two different mechanisms: `transients` and the `WP_Object_Cache` class. The hallmark of object caching is to provide a persistent caching backend that allows cached data to be available across requests. Both `transients` and the `WP_Object_Cache` class can provide this persistence.

### **2. Transients**

Out of the box, WordPress supports persistent object caching via transients. The WordPress transients API allows you to store, retrieve, and delete objects from the database. By default, the transients cache uses the `wp_options` table for data storage. A point of confusion regarding transients as an object cache often comes from fact that transients are stored in WordPress’s MySQL database table. A MySQL database is a minimally sufficient place to store objects as it can be a location that allows much faster retrieval of data than the object’s original location.

Transients are an excellent object cache option in WordPress because they provide a persistent cache with zero configuration. For plugin and theme developers, you can nearly guarantee that you will be using a persistent cache when you use the transients API. The downside to the transient cache is that it is using MySQL, which is one of the slower and riskier options for storing cached data. It is usually a better strategy to separate the caching engine from the main data store in order to maximize the efficiency of both stores.

### **3. The WP\_Object\_Cache Class**

The [WP\_Object\_Cache](https://developer.wordpress.org/reference/classes/wp_object_cache/) class is a class that defines the storage engine for WordPress’s object cache. This class can be overridden with a custom class, meaning that a developer can configure WordPress to use any storage engine as an object cache. The two most popular in the WordPress world is Memcached and Redis.

The advantage of using the WP\_Object\_Cache class is primarily performance. Using this class allows you to extend WordPress to use the absolute best caching engines in the world. For instance, using Memcached as WordPress’s object cache gives ridiculously fast data access that easily scales to multiple servers. Memcached is an important caching engine for use with high traffic websites. With the WP\_Object\_Cache class, developers can finely tune the caching experience in WordPress, whereas using the transients API gives you very little control over the caching engine. Relating this class back to the metaphor, the WP\_Object\_Cache class allows you to precisely decide where your food will be stored.

As an added benefit of using the WP\_Object\_Cache class, code that uses the transient API will actually use the storage engine in WP\_Object\_Cache class if it is defined. For instance, if you have a finely tuned system using Memcached as the caching engine and you install a plugin that uses the transients API, it will take full advantage of your Memcached installation instead of storing data in the MySQL database.

By default, the WP\_Object\_Cache is defined, but only implements a run-time cache. Since WordPress cannot decide whether or not you have a storage engine available and because it has to make sure that use of the object cache API does not cause fatal errors, it implements a default WP\_Object\_Cache class. This default class merely stores data in a PHP variable during run time and is non-persistent. This, however, can be overridden.

To define your own object cache, you must add a file to `wp-content/` named `object-cache.php`. If this file is defined, it will be loaded instead of the default class. This type of file is known as a WordPress drop-in. A few different `object-cache.php` files exist in the plugin repository for different caching engines like Memcached, Redis or OPcache.

### 4. WordPress Page Caching

Similar to the WP\_Object\_Cache class, WordPress offers a drop-in to define a page cache. By placing a file named `advanced-cache.php` into the `wp-content/` directory, you can define all of the logic related to caching a page.

The general idea for the logic behind the page caching mechanism is as follows:

1. Based on the URL of the request \(as well as a few other pieces of information\), look in the object cache to see if the cached version of the page exists.
2. If the page exists, serve it and complete the request.
3. If the page does not exist, start output buffering, load the page, finish output buffering and store the output for subsequent requests.

There are some finer nuances to a page caching system, but that logic defines the main mechanism for generating a page cache.

The beauty of `advanced-cache.php` is that it is loaded in the first 1% of the WordPress page load. As such, if the cached page is found, 99% of the WordPress load is avoided, which leads to a significant performance boost in the application. An important thing to note with `advanced-cache.php` is that it needs a persistent cache to store its data. The most effective solution is to use a persistent object cache as the data store, but solid solutions exist that utilize the file system for this cache.

While use of `advanced-cache.php` is the easiest and most accessible form of page caching in WordPress, you can also use a reverse proxy approach with Varnish, Nginx, or a hosted caching solution. We will not go into these solutions here because these are mostly configured at the systems level and have little to do with WordPress specifically. We only touch on this as an alternative to `advanced-cache.php`.

## Caching Tips

Now that you know a little about caching in WordPress, here some nuggets of "wisdom" that has been collected through experience with caching in WordPress. In hope, you can avoid some struggles that some people have faced before.

1. Never depend on your cache for application functionality. You should always develop your application with the assumption that the cache is 100% broken. Whether or not the cache is operational, your application should still provide the intended functionality. Your cache should always be able to regenerate itself if it is corrupted.
2. Use your cache as a "progressive enhancement" for performance. While your application should function without the cache, that does not mean that it should perform well. The caching layer is to provide better performance, not functionality. As such, you can think of the caching layer as a progressive enhancement that improves the performance of the application but still works without caching.
3. Test your application with caching on and off. To verify that your application is functioning properly, it should be tested with and without the cache turned on. Depending on the caching strategy and storage engines used this can be more or less difficult. To avoid really inconvenient surprises, it is best to check the application in both states.
4. Always set an expiration value for every object that is cached. Theoretically, for maximum efficiency, you should only refresh a cached object when it changes; however, you will eventually stumble upon a very difficult to debug situation if you do not set your cached objects to eventually expire. This will help avoid issues of stale data being served.
5. Know the system that you are caching for. Different caching strategies can be used only if certain requirements are met by the environment. It is always best to learn as much about the environment as possible before building the application.

## Conclusion

In this article, we discussed the essential concepts that one must understand in order to be able to apply caching to a web development project. With this information, in hope, you are better prepared to work with caching in your projects. This article is intended to serve as a primer that makes it easier to understand more complex caching concepts.



{% hint style="success" %}
This article originally from [tollmanz.com](https://www.tollmanz.com/core-caching-concepts-in-wordpress/) blog.
{% endhint %}



