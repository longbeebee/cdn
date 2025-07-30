# CDN
## Introduction to CDN

### What is CDN ? 

![cdn-1](cdn-architecture.png)

CDN (Content Delivery Network) is the distributed content network, which is all systems placed worldwide. It is used to deliver images, web, video, files,... to users with the highest performance.

<strong>How it's work ?</strong>

It could cache all content when users have access to the website, and the content would be loading directly at the nearest server instead of loading from the origin server. So that the server has increased workload and decreased resources.

In this guide, we would apply the CDN to address the performance problem that we met in our website, and we had chosen CloudFlare.

<strong>Let's begin the journey !!!</strong>

### The Problem

At runtime, there is so much static data loaded by the origin server. It used most of the pages, which is why users have bad UX.

To issue that, we would set up the middleware server to cache all static models and deliver them directly.

### Setup The CloudFlare

Firstly, we must be assigning the domain to CloudFlare's nameservers.

![cdn-2](cdn-01.png)

Secondly, we configure the DNS. Let's create some records with type A and IP address. After that, we turn on the proxy status.

![cdn-3](cdn-02.png)

Thirdly, we create the caching rules for the API. For example, we set the rules for the content proxy API in our system.

![cnd-4](cdn-03.png)

![cdn-5](cdn-04.png)

and make conditions that apply for the response header, including the cache-control header.

![cdn-6](cdn-05.png)

### Create a new API

We would create a new API compatible with all conditions in Spring Boot 2.7.x.

<strong>Why do we do that?. Obviously, we had gotten the static images from the other site and met the CORS error. To ignore that, we must get the data and convert another header from the origin server to our sites.</strong>

![cdn-7](cdn-06.png)


### Test Performance

![cdn-8](cdn-07.png)

the first access, we see that the images are firstly loaded from the origin server. The response header includes:
- cache-control: public, max-age=86400, stale-while-revalidate=86400
- cf-cache-status: MISS (not cached)
- server: cloudflare

average loading time: ~2s

![cdn-9](cdn-08.png)

the second access, we see that the images are loaded from Cloudflare. The response header includes:
- cache-control: public, max-age=86400, stale-while-revalidate=86400
- cf-cache-status: HIT (cached)
- server: cloudflare

![cdn-10](cdn-09.png)

average loading time: ~300 millisecond

![cdn-11](cdn-10.png)

### Conclusion

Successfully, we have already applied CDN to solve the performance problem when loading the big resources. We still applied the solution to video, CSS files, and more ... :D Thanks for reading this document.













