---
layout: post
title:  "iOS image caching. Libraries benchmark (SDWebImage vs FastImageCache)"
date:   2014-03-21 12:26:18.000000000+02:00
comments_id: 5
categories: 
tags: [AFNetworking, background, cache, caching, decompression, disk, FastImageCache, Haneke, image, memory, SDWebImage, TMCache]


---

<img style="float: right;" width="30%" src="/assets/photos_time_screen.jpg">

# 1. Introduction

In the past years, iOS apps have become more and more visually appealing. Displaying images is a key part of that, that's why most of them use images that need to be downloaded and rendered. Most developers have faced the need to populate table views or collection views with images. Downloading the images is resource consuming (cellular data, battery, CPU, ...), so in order to minimize this the caching model appeared.

To achieve a great user experience, it's important to understand what is going on under the iOS hood when we cache and load images.

Also, the benchmarks on the most used image caching open source libraries can be of great help when choosing your solution.

# 2. Classical approach

-   download the images asynchronously
-   process images (scale, remove red eyes, remove borders, ...) so they are ready to be displayed
-   write them on disk
-   read from disk and display them when needed

```objective-c
// assuming we have an NSURL *imageUrl and UIImageView *imageView, we need to load the image from the URL and display it in the imageView
if ([self hasImageDataForURL:imageUrl] {
  NSData *data = [self imageDataForUrl:imageUrl];
  UIImage *image = [UIImage imageWithData:imageData];
  dispatch_async(dispatch_get_main_queue(), ^{
    imageView.image = image;
  });
} else {
  [self downloadImageFromURL:imageUrl withCompletion:^(NSData *imageData, …) {
    [self storeImageData:imageData …];
    UIImage *image = [UIImage imageWithData:imageData];
    dispatch_async(dispatch_get_main_queue(), ^{
      imageView.image = image;
    });
  }];
}
```

#### FPS simple math:

-   *60 FPS* is our ideal for any UI update, so the experience is flawless
-   *60 FPS* =\> *16.7 ms per frame*. This means that if any main-queue operation takes longer than *16.7 ms*, the scrolling FPS will drop, since the CPU will be busy doing something else than rendering UI.

# 3. Downsides of the classical variant:

-   **loading images** or any file **from the disk is expensive** (disk access is usually from 10.000 to 1.000.000 times slower than memory access. See comparison [here](http://www.storagereview.com/introduction_ram_disks). If we refer to SSD disks, those can come closer to RAM speeds (like 10 times slower), but at this point no smartphone or tablet is equipped with an SSD unit.
-   creating the **`UIImage` instance** will result in a **compressed version of the image mapped to a memory section**. The compressed image is small and cannot be rendered. If loaded from disk, the image is not even loaded into memory. **Decompressing** an image is also **expensive**.
-   setting the image property of the imageView in this case will create a `CATransaction` that will be committed on the run loop. On the next run loop iteration, the `CATransaction` involves (depending on the images) creating a **copy** of any **images** which have been set as layer contents. Copying images includes:
    -   **allocating buffers** for file IO and decompression
    -   **reading disk data** into memory
    -   **decompressing** the image data (results the raw bitmap) - **high CPU consumer**
    -   **`CoreAnimation`** uses the decompressed data and **renders** it
-   **improper byte-aligned images** are **copied** by **`CoreAnimation`** so that their byte-alignament is fixed and can be rendered. This isn't stated by Apple docs, but profiling apps with Instruments shows `CA::Render::copy\_image` even when the `Core Animation` instrument shows no images copied
-   starting with **iOS 7**, the **JPEG** **hardware** **decoder** is **no longer accessible** to 3rd party apps. This means our apps are relying on a software decoder which is significantly slower. This was noticed by the **`FastImageCache` team** on their [Github page](https://github.com/path/FastImageCache#the-problem) and also by __Nick Lockwood__ on a [Twitter post](https://twitter.com/nicklockwood/status/401128101049942016).

# 4. A strong iOS image cache component must:

-   **download** images **asynchronously**, so the main queue is used as little as possible
-   **decompress** images **on a background queue**. This is far from being trivial. See a strong article about [background decompression](http://www.cocoanetics.com/2011/10/avoiding-image-decompression-sickness/)
-   **cache** images into **memory** and on **disk**. Caching on disk is important because the app might be closed or need to purge the memory because of low memory conditions. In this case, re-loading the images from disk is a lot faster than downloading them. Note: if you use `NSCache` for the memory cache, this class will purge all it's contents when a memory warning is issued. Details about `NSCache` here <http://nshipster.com/nscache/>
-   **store** the **decompressed** **image** on disk and in memory to avoid redoing the decompression
-   use **GCD** and **blocks**. This makes the code more performant, easier to read and write. In nowadays, GCD and blocks is a must for async operations
-   *nice to have*: *category over `UIImageView`* for trivial integration.
-   *nice to have: ability to process the image after download and before storing it into the cache.*

#### Advanced imaging on iOS

To find out more about imaging on iOS, how the SDK frameworks work (`CoreGraphics`, `Image IO`, `CoreAnimation`, `CoreImage`), CPU vs GPU and more, go through this [great article by \@rsebbe](http://www.slideshare.net/rsebbe/2014-cocoaheads-advimaging).

#### **Is Core Data a good candidate?**

Here is a [benchmark of image caching using Core Data versus File System](http://biasedbit.com/filesystem-vs-coredata-image-cache/), the results are recommending the File System (as we are already accustomed to).

Just looking at the concepts listed above makes it clear that writing such a component on your own is hard, time consuming and painful. That's why we turn to open source image caching solutions. Most of you have heard of `SDWebImage` or the new `FastImageCache`. In order to decide which one fits you best, I've benchmarked them and analysed how they match our list of requirements.

# 5. Benchmarks

### Libraries tested:

-   [SDWebImage](https://github.com/rs/SDWebImage) - version [3.5.4](https://github.com/rs/SDWebImage/releases/tag/3.5.4)
-   [FastImageCache](https://github.com/path/FastImageCache) - version [1.2](https://github.com/path/FastImageCache/releases/tag/1.2)
-   [AFNetworking](https://github.com/AFNetworking/AFNetworking) - version [2.2.1](https://github.com/AFNetworking/AFNetworking/releases/tag/2.2.1)
-   [TMCache](https://github.com/tumblr/TMCache/) - version [1.2.0](https://github.com/tumblr/TMCache/releases/tag/1.2.0)
-   [Haneke](https://github.com/hpique/Haneke) - version [0.0.5](https://github.com/hpique/Haneke/releases/tag/v0.0.5)

*Note: `AFNetworking` was added to the comparison because it benefits of disk caching from iOS 7 (due to `NSURLCache`).*

#### Scenario:

\- for each library, I made a clean install of the benchmark app, then started the app, scroll easily while all images are loaded, then scroll back and forth with different intensities (from slow to fast). I closed the app to force loading from disk cache (where available), then run the same scrolling scenario.

#### Benchmark app - project:

\- the demo project source can be found on Github under the name [ImageCachingBenchmark](https://github.com/bpoplauschi/ImageCachingBenchmark), together with the charts, collected data tables and more.\
- please note the project from Github had to be modified as well as the image caching libraries so that we know the cache source of each image loaded. Because I didn't want to check in the Cocoapods source files (not a good practice) and that the project code must compile after a clean install of the Cocoapods, the current version of the Github project is slightly different from the one I used for the benchmarks.
- if some of you want to rerun the benchmarks, you need to make a similar `completionBlock` for image loading for all libraries like the default one on `SDWebImage` that returns the `SDImageCacheType`.

#### Fastest vs slowest device results

[Complete benchmark results](http://htmlpreview.github.io/?https://github.com/bpoplauschi/ImageCachingBenchmark/blob/master/tables/tables.html) can be found on the Github project. Since those tables are big, I decided to create charts using the fastest device data (iPhone 5s) and the slowest (iPhone 4).

##### iPhone 5s results

![iPhone 5s CPU and mem](/assets/iPhone5s_cpu_mem.png)

![iPhone 5s disk](/assets/iPhone5s_disk.png)

![iPhone 5s FPS](/assets/iPhone5s_FPS.png)

![iPhone 5s mem](/assets/iPhone5s_mem.png)

##### iPhone 4 results

![iPhone 4 CPU and mem](/assets/iPhone4_cpu_mem.png)

![iPhone 4 disk](/assets/iPhone4_disk.png)

![iPhone 4 FPS](/assets/iPhone4_FPS.png)

![iPhone 4 mem](/assets/iPhone4_mem.png)

## Summary

| Results           | SDWebImage | FastImageCache | AFNetworking | TMCache | Haneke |
| ----------------- | ---------- | -------------- | ------------ | ------- | ------ |
| async download    | ✓          | ✗              | ✓            | ✗       | ✓      |
| backgr decompr    | ✓          | ✓              | ✗            | ✗       | ✗      |
| store decompr     | ✓          | ✓              | ✗            | ✗       | ✗      |
| memory cache      | ✓          | ✓              | ✓            | ✓       | ✓      |
| disk cache        | ✓          | ✓              | NSURLCache   | ✓       | ✓      |
| GCD and blocks    | ✓          | ✓              | ✓            | ✓       | ✓      |
| easy to use       | ✓          | ✗              | ✓            | ✗       | ✓      |
| UIImageView categ | ✓          | ✗              | ✓            | ✗       | ✓      |
| from memory       | ✓          | ✗              | ✓            | ✗       | ✓      |
| from disk         | ✗          | ✓              | ✗            | ✗       | ✗      |
| lowest CPU        | ✓          | ✗              | ✗            | ✗       | ✓      |
| lowest mem        | ✓          | ✓              | ✗            | ✗       | ✓      |
| high FPS          | ✓          | ✓              | ✓            | ✗       | ✓      |
| License           | MIT        | MIT            | MIT          | Apache  | Apache |

#### Table legend:
- async download = support for asynchronous downloads directly into the library
- backgr decompr = image decompression executed on a background queue / thread
- store decompr = images are stored in their decompressed version
- memory  /disk cache = support for memory / disk cache
- `UIImageView` categ = category for `UIImageView` directly into the library
- from memory / disk = top results for the average retrieve times from memory / disk cache

# 6. Conclusions

- writing an iOS image caching component from scratch is hard
- `SDWebImage` and `AFNetworking` are solid projects, with many contributors, that are maintained properly. `FastImageCache` is catching up pretty fast with that.
- looking at all the data provided above, I think we can all agree **`SDWebImage`** is the best solution at this time, even if for some projects `AFNetworking` or `FastImageCache` might fit better. It all depends on the project\'s requirements.
