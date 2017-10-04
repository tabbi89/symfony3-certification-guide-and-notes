# HTTP Caching

#### Certification requirements

* [Cache types (browser, proxies and reverse-proxies)](#cache-types)
* [Expiration (Expires, Cache-Control)](#expiration)
* [Validation (ETag, Last-Modified)](#validation)
* Client side caching
* Server side caching
* [Edge Side Includes](#esi)

#### Extras

* [Summary of all cache Headers](#summary-cache-header)
* [Cache HTTP status codes](#summary-http-status)
* [Cacheable HTTP methods](#summary-http-methods)
* [Summary of Symfony Response cache methods API](#summary-symfony-response)
* [Katas](#summary-katas)
 
## Cache types <a id="cache-types"></a>

**What are Caches?**
At a high-level, there are two popular types of caching:

* In-app caching (APC, Memcache, Redis, other memory stores)
* HTTP caching - proxy, gateway, and private

**In-App**
In-app caching lives in your infrastructure and interacts with your application code. It allows you to store objects in memory for quick retrieval. It takes action when a request makes its way to your server. Your code checks your in-app caches and determines if it should use them to retrieve data in order to fulfill the request.

**HTTP Caching** is mostly done by third-parties. They can potentially save requests from ever reaching your server by responding to a request itself.

* **Proxy** - A proxy cache is a public, shared cache often employed by an ISP or large corporation. Because they are employed at a high-level, they can (and do) cache thousands of various websites.
* **Gateway** - Similar to in-app caches, Gateway caches live as part of your infrastructure. They sit in front of your web-server and act much in the same way of proxy servers - except they are for your application(s) only. They are technically *reverse proxies, surrogate caches, or even HTTP accelerators*
* **Private** - Private caches are caches that are unique to a specific user. They live on the client-end. Your browser is a private cache; it will cache responses unique to the sites you visit.
Making a response (web page, api-response, etc) cacheable is part of the HTTP cache mechanism. Which cache mechanisms you use depends on your use case.

[source - and more about cache types](http://fideloper.com/quick-caching-explanation)

#### You should know

* [what is a browser (private) cache][1000]
* [what is a reverse-proxy (gateway) cache][1000]
* [what is a proxy cache][1000]
* [where lives proxy cache][1000]
* [where lives reverse-proxy cache][1000]
* [where lives browser cache][1000]
* [the difference between in-app cache and HTTP caching][1000]
* [that symfony comes with built in reverse-proxy cache][1001]
* [that in Standard Symfony Edition where you have enabled "framework.http_method_override" option you have to enable it also in front controller Request::enableHttpMethodParameterOverride()][1002]
* [that to get information what happened in the cache layer in AppCache you have to print error_log($kernel->getLog())][1003]
* [how to add default cache configuration to AppCache][1004]
* [what are options for cache configuration in AppCache][1005]
* [that you can without any problems and any extra configuration switch to Varnish and another reverse proxy cache][1006]
* [that HTTP Caching including description of all cache headers is available in RFC 2616][1007]
* that to enable Symfony Standard Edition built in reverse proxy you have to wrap the AppKernel in AppCache
* [that in debug mode symfony automatically adds X-Symfony-Cache header to the response to get information about hits and misses][1008]
* [how with default Varnish configuration correctly forward IP in Symfony reverse proxy][1009]
* that to trust all X-Forwarded-* headers you have to set it as Request::setTrustedProxies(Request::HEADER_X_FORWARDED_ALL)

[1000]: http://fideloper.com/quick-caching-explanation
[1001]: https://symfony.com/doc/current/http_cache.html#symfony-reverse-proxy
[1002]: https://symfony.com/doc/current/reference/configuration/framework.html#configuration-framework-http-method-override
[1003]: https://symfony.com/doc/current/http_cache.html#symfony-reverse-proxy
[1004]: http://api.symfony.com/3.3/Symfony/Bundle/FrameworkBundle/HttpCache/HttpCache.html#method_getOptions
[1005]: http://api.symfony.com/3.0/Symfony/Component/HttpKernel/HttpCache/HttpCache.html#method___construct
[1006]: https://symfony.com/doc/current/http_cache/varnish.html
[1007]: https://tools.ietf.org/html/rfc2616
[1008]: https://symfony.com/doc/3.0/http_cache.html#symfony-reverse-proxy
[1009]: http://symfony.com/doc/3.0/http_cache/varnish.html#make-symfony-trust-the-reverse-proxy 

### Expiration <a id="expiration"></a>

#### You should know

* [that expiration model is the most efficient and straightforward][10000]
* that expiration model is over validation model
* [that you can use both expiration and validation model][10000]
* [that you can define HTTP cache headers using annotation][10001]
* [what are headers for expiration][10002]
* [what method should be call to set Cache-Control header for expiration][10004]
* [that $response->setExpires(DateTime $date = null) automatically converts to GMT timezone][10003]
* [that expiration model saves round-trips to the origin server][10005]

[10000]: https://symfony.com/doc/3.0/http_cache/expiration.html
[10001]: https://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/annotations/cache.html
[10002]: https://symfony.com/doc/3.0/http_cache/expiration.html#expiration-with-the-cache-control-header
[10003]: https://symfony.com/doc/3.0/http_cache/expiration.html#expiration-with-the-expires-header
[10004]: https://symfony.com/doc/3.0/http_cache/expiration.html#expiration-with-the-cache-control-header
[10005]: http://fideloper.com/quick-caching-explanation

### Validation <a id="validation"></a>

* [what is the difference between validation and expiration model][val-1]
* that validation model saves CPU
* [what mean 304 Status code and what is returned in response body][https://symfony.com/doc/3.0/http_cache/validation.html]
* that there are ETag and Last-Modified headers in validation model
* [how in Symfony looks validation with ETag header][val-2]
* [how in Symfony looks validation with Last-Modified header][val-3]
* [what method is used to compare with ETag and Last-Modified headers][val-4]
* that If-None-Match is compared with ETag and If-Modified-Since is compared with Last-Modified
* [how to optimize code with validation model][val-5]

[val-1]: https://symfony.com/doc/3.0/http_cache/validation.html
[val-2]: https://symfony.com/doc/3.0/http_cache/validation.html#validation-with-the-etag-header
[val-3]: https://symfony.com/doc/3.0/http_cache/validation.html#validation-with-the-last-modified-header
[val-4]: http://api.symfony.com/3.0/Symfony/Component/HttpFoundation/Response.html#method_isNotModified
[val-5]: https://symfony.com/doc/3.0/http_cache/validation.html#optimizing-your-code-with-validation

### Client side caching
### Server side caching

### Edge Side Includes <a id="esi"></a>

#### You should know

* [that to avoid caching whole page limitation in gateway caches you should use ESI to cache only parts][esi-1]
* that there is only one esi tag implemented in symfony `include` - `<esi:include src="http://..." />`
* that ESI tag requires a fully-qualified URL
* that ESI tag represents a page fragment that can be fetched via the given URL
* [how to enable esi in Symfony Standard Edition][esi-2]
* [that Symfony will fallback to `render` in `render_esi` if no gateway cache is installed]
* [what are helpers to render esi tags in twig][esi-3]
* that variables passed to `render_esi` become part of the cache key so that you have unique caches for each combination of variables and values
* that once you start using ESI, remember to always use the s-maxage directive instead of max-age. As the browser only ever receives the aggregated resource, it is not aware of the sub-components, and so it will obey the max-age directive and cache the entire page
* [what options supports `render_esi`][esi-4]

[esi-1]: https://symfony.com/doc/3.0/http_cache/esi.html
[esi-2]: https://symfony.com/doc/3.0/http_cache/esi.html#using-esi-in-symfony
[esi-3]: https://symfony.com/doc/3.0/http_cache/esi.html#using-esi-in-symfony
[esi-4]: https://symfony.com/doc/current/reference/twig_reference.html#render-esi

### Summary of all cache Headers <a id="summary-cache-headers"></a>

#### Cache request directives

* Cache-Control: max-age=<seconds>
* Cache-Control: max-stale[=<seconds>]
* Cache-Control: min-fresh=<seconds>
* Cache-Control: no-cache 
* Cache-Control: no-store
* Cache-Control: no-transform
* Cache-Control: only-if-cached

#### Other

* If-None-Match: "bfc13a64729c4290ef5b2c2730249c88ca92d82d"
* If-None-Match: *
* If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT

#### Cache response directives

* Cache-Control: must-revalidate
* Cache-Control: no-cache
* Cache-Control: no-store
* Cache-Control: no-transform
* Cache-Control: public
* Cache-Control: private
* Cache-Control: proxy-revalidate
* Cache-Control: max-age=<seconds>
* Cache-Control: s-maxage=<seconds>

* Expires: Wed, 21 Oct 2015 07:28:00 GMT

The Cache-Control header is defined in HTTP/1.1 specification and supersedes Expires header.
**If there is a Cache-Control header with the "max-age" or "s-max-age" directive in the response, the Expires header is ignored.**

#### Other

* Vary: Accept-Encoding, User-Agent
* Last-Modified: Wed, 21 Oct 2015 07:28:00 GMT
* ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"

#### Examples

##### Preventing caching

`Cache-Control: no-cache, no-store, must-revalidate`

##### Caching static assets

`Cache-Control: public, max-age=31536000`

#### You should know

* [that to disable caching, it means forcing to submit request to the origin server you should use no-store][1]
* [the difference between public private directives][2]
* [the difference between no-cache and no-store][3]
* [the difference between max-age and s-maxage directives][4]
* [how to revalidate and reload cache using headers][5]
* [what response header is compared with If-Non-Match request header][6]
* [if ETag and If-None-Match headers are matched 304 Not Modified without response body is send][7]
* [what response header is compared with If-Modified-Since request header][8]
* [that if If-Modified-Since and Last-Modified headers are matched response will be 304 Not Modified without body][9]
* [that If-Modified-Since can only be used with a GET or HEAD methods][10]
* [that HTTP specifies 4 cache headers Cache-Control Expires ETag Last-Modified][11]
* [what cache headers belong to Expiration model][12]
* [what cache headers belong to Validation model][13]
* [how to vary response for HTTP Cache][14]

[1]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control#Other
[2]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control#Cacheability
[3]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control#Cacheability
[4]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control#Expiration
[5]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control#Revalidation_and_reloading
[6]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#Caching_of_unchanged_resources
[7]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#Caching_of_unchanged_resources
[8]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since
[9]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since
[10]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since
[11]: https://symfony.com/doc/current/http_cache.html#making-your-responses-http-cacheable
[12]: https://symfony.com/doc/3.0/http_cache/expiration.html
[13]: https://symfony.com/doc/3.0/http_cache/validation.html
[14]: https://symfony.com/doc/3.0/http_cache/cache_vary.html


### Cache HTTP status codes <a id="summary-http-status"></a>

According to RFC 7231 (HTTP/1.1) HTTP status codes are defined as cacheable unless otherwise indicated by the method definition or explicit cache controls:

* 200 OK
* 203 Non-Authoritative Information
* 204 No Content
* 206 Partial Content
* 300 Multiple Choices
* 301 Moved Permanently
* 404 Not Found
* 405 Method Not Allowed
* 410 Gone
* 414 URI Too Long
* 501 Not Implemented

All other status codes are not cacheable by default.

### Cacheable HTTP methods <a id="summary-http-methods"></a>

* POST
* GET
* HEAD

**Responses to POST requests are only cacheable when they include explicit freshness information** [see more](https://www.mnot.net/blog/2012/09/24/caching_POST)

#### You should know

* what methods are cacheable
* does POST method can be cachable without any extra information


### Summary of Symfony Response cache methods API <a id="summary-symfony-response"></a>

The Response class has a rich set of methods to manipulate the HTTP headers related to the cache:

* setPublic();
* setPrivate();
* expire();
* setExpires();
* setMaxAge();
* setSharedMaxAge();
* setTtl();
* setClientTtl();
* setLastModified();
* setEtag();
* setVary();
* setCache();

Other methods:

* isNotModified($request);
* mustRevalidate()

#### You should know

* [what options can be passed to `setCache(array $options)`][100]
* [what method method validates ETag and Last-Modified][101]
* [that setEtag have optional attribute bool $weak = false which makes ETag weak][102]
* [that passing null to setLastModified(DateTime $date = null) will remove the header][103]
* [that setExpires(DateTime $date = null) will automatically convert to GTM timezone][104]
* that setLastModified(DateTime $date = null) will automatically convert to GTM timezone
* that passing null to setLastModified(DateTime $date = null) will automatically remove header
* [that setTtl(int $seconds) adjusts the Cache-Control/s-maxage directive.][105]
* [that setClientTtl(int $seconds) adjusts the Cache-Control/max-agedirective.][106]

[100]: https://symfony.com/doc/3.0/components/http_foundation.html#managing-the-http-cache
[101]: https://symfony.com/doc/3.0/components/http_foundation.html#managing-the-http-cache
[102]: http://api.symfony.com/3.0/Symfony/Component/HttpFoundation/Response.html#method_setEtag
[103]: http://api.symfony.com/3.0/Symfony/Component/HttpFoundation/Response.html#method_setLastModified
[104]: https://symfony.com/doc/3.0/http_cache/expiration.html#expiration-with-the-expires-header
[105]: http://api.symfony.com/3.0/Symfony/Component/HttpFoundation/Response.html#method_setTtl
[106]: http://api.symfony.com/3.0/Symfony/Component/HttpFoundation/Response.html#method_setClientTtl

### Katas <a id="summary-katas"></a>

1. Setup Symfony application with Varnish as Reverse Proxy cache. Use ESI to play with caching responses
2. Enable ESI in Symfony application and play with ESI helpers and raw esi tag. See what happens when you disable Symfony Reverse Proxy cache
3. Play with all API response methods for expiration and validation model. Check that expiration is over validation
4. Learn how to debug Symfony Reverse Proxy (X-Symfony-Cache header)
5. Play with cache invalidation