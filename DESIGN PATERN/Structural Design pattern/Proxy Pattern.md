
#### Problem It Solves

It solves the problem of **uncontrolled or expensive access** to an object. **For example, consider a scenario where:**

- You have a heavy object like a video player that consumes a lot of resources on initialization.
- You want to delay its creation until it's actually needed (lazy loading).
- Or maybe the object resides on a remote server and you want to add a layer to manage the network communication.

  
The Proxy Pattern allows you to **control access, defer initialization, add logging, caching, or security** without modifying the original object.   
  

---

## Real-Life Coding Example

Imagine you're building a feature like a **video streaming app** (think YouTube or Netflix) where users can download videos. Now, consider this: multiple users might try to download the **same video**multiple times - or even the **same user** may repeat the request. In such scenarios, if we go ahead and download the video from the internet every single time, it leads to **unnecessary network calls, longer wait times,** and **wasted bandwidth**.   
  
Let’s consider a scenario where we want to **download a video multiple times**, perhaps from different places in the code or by different users. A poor design would look like this:

#### Bad Design: Without Proxy

Java

```java
// ========== RealVideoDownloader Class ==========
class RealVideoDownloader {
    public String downloadVideo(String videoUrl) {
        // caching logic missing
        // filtering logic missing
        // access logic missing
        System.out.println("Downloading video from URL: " + videoUrl);
        String content = "Video content from " + videoUrl;
        System.out.println("Downloaded Content: " + content);
        return content;
    }
}

// ================ Main Class ===================
class Main {
    public static void main(String[] args) {
        System.out.println("User 1 tries to download the video.");
        RealVideoDownloader downloader1 = new RealVideoDownloader();
        downloader1.downloadVideo("https://video.com/proxy-pattern");

        System.out.println();

        System.out.println("User 2 tries to download the same video again.");
        RealVideoDownloader downloader2 = new RealVideoDownloader();
        downloader2.downloadVideo("https://video.com/proxy-pattern");
    }
}
```

###### Understanding the Issues

- There's **no caching**, so the same video is downloaded again and again even if it’s already available.
- There's **no access control** or **content filtering** - any video URL is downloaded without restrictions.
- The **client directly depends** on the RealVideoDownloader, meaning there’s no way to intercept, log, or modify the download behavior without changing core logic.
- It results in **multiple object creations** and **redundant resource usage**.

  
The previous implementation made **direct use of the `RealVideoDownloader` class for every download request**, even if the same video was requested multiple times. This meant the system would **re-download and reprocess the same video repeatedly**, leading to **unnecessary network usage and redundant computation**.   
  
To solve this, we use the **Proxy Design Pattern** - a structural pattern that provides a **placeholder** or **surrogate for another object** to control access to it.   
  

#### Good Design: Using Proxy Pattern

Java

```java
import java.util.*;

interface VideoDownloader {
    String downloadVideo(String videoURL);
}

// ========== RealVideoDownloader Class ==========
class RealVideoDownloader implements VideoDownloader {

    @Override
    public String downloadVideo(String videoUrl) {
        System.out.println("Downloading video from URL: " + videoUrl);
        return "Video content from " + videoUrl;
    }
}

// =============== Proxy With Cache ====================
class CachedVideoDownloader implements VideoDownloader {

    private RealVideoDownloader realDownloader;
    private Map<String, String> cache;

    public CachedVideoDownloader() {
        this.realDownloader = new RealVideoDownloader();
        this.cache = new HashMap<>();
    }

    @Override
    public String downloadVideo(String videoUrl) {
        if (cache.containsKey(videoUrl)) {
            System.out.println("Returning cached video for: " + videoUrl);
            return cache.get(videoUrl);
        }

        System.out.println("Cache miss. Downloading...");
        String video = realDownloader.downloadVideo(videoUrl);
        cache.put(videoUrl, video);
        return video;
    }
}


// ================ Main Class ===================
class Main {
    public static void main(String[] args) {
        VideoDownloader cacheVideoDownloader = new CachedVideoDownloader();
        System.out.println("User 1 tries to download the video.");
        cacheVideoDownloader.downloadVideo("https://video.com/proxy-pattern");

        System.out.println();

        System.out.println("User 2 tries to download the same video again.");
        cacheVideoDownloader.downloadVideo("https://video.com/proxy-pattern");
    }
}
```

#### How Proxy Pattern Solves the Issue

In this example, the **proxy (`CachedVideoDownloader` Class)** was used that implements a **caching logic** that checks if a video has already been downloaded. If so, it simply returns the **cached result**, saving time and resources.   
  
This way, the **real object is accessed only when absolutely necessary**, while repeated requests are served **efficiently through the proxy** - resulting in faster response times and optimized performance.   
  

---

## When to Use Proxy Pattern?

The proxy pattern can be used when: - **When object creation is expensive**, and you want to delay or control its instantiation.
- **When you need to control access** to sensitive operations or enforce permission checks.
- **When interacting with remote objects** that are costly or slow to fetch.
- **When lazy loading is needed** to optimize system performance and resource usage.