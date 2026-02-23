---
title: zncut the fastest way to remove ads from local videos
date: 2026-02-21
featured_image: /images/blog/zncut/hero-social.png
excerpt: The fastest command line tool for removing ads from local videos. Frame-accurate cuts in under 10 seconds, no matter how long the video.
tags:
  - blog
  - code
  - ossposts
---

I'm not saying you should use [yt-dlp](https://github.com/yt-dlp/yt-dlp) ( an open source tool that downloads YouTube videos) but if you do, pair it with [zncut](https://github.com/zincplusplus/zncut). It's the fastest command line tool for removing ads from local videos. It's a wrapper around [ffmpeg](https://ffmpeg.org/) that uses [SponsorBlock](https://sponsor.ajay.app/) to identify the segments automatically and **cuts precisely on the frame in under 10s**.

Here's how I made it so fast.

<p class="text-center">
  <img class="rounded-xl" src="/images/blog/zncut/hero.png"/>
</p>

Here's your video, 30 minutes, with an ad segment of 4 minutes. You have keyframes every 1 minute ( in reality they vary between 2-10 seconds).

<p class="text-center">
  <img class="rounded-xl" src="/images/blog/zncut/encode-everything.png"/>
</p>

If I'd cut on the lines using something like `ffmpeg` it takes my machine 1 minute of re-encoding for 1 minute of video. No matter how short the segment you want to remove is, you will have to reencode the whole video. Imagine doing this for a 2h podcast. **We can do better.**

## Cut on keyframes

Cutting on keyframes doesn't require re-encoding because keyframes are complete, self-contained images. Everything between keyframes is just the difference from the last one. When you cut on a keyframe, the video player already knows how to start from there. No reconstruction needed.

That's why cutting on keyframes is super fast. We're talking 1-2seconds per video. But your segments won't align with the keyframes most of the times. So you have two strategies:

<p class="text-center">
  <img class="rounded-xl" src="/images/blog/zncut/overcut.png"/>
  Overcut and lose part of the content
</p>

<p class="text-center">
  <img class="rounded-xl" src="/images/blog/zncut/undercut.png"/>
  Undercut and see up to 20s of ads.
</p>

Both options are **fast** but their accuracy is **unacceptable**.

## Best of both worlds

[zncut](https://github.com/zincplusplus/zncut) does something smarter. It breaks the video in 5 segments, all cut on keyframes for speed.

<p class="text-center">
  <img class="rounded-xl" src="/images/blog/zncut/zncut.png"/>
</p>

- it keeps the green segments (1 and 5) as is;
- segment 3, the bulk of the ad is dropped;
- it re-encodes segments 2 and 4 by creating keyframes around the cut lines;
- stitches the 4 remaining segments into the final video.

By combining the best of both worlds we spend 10s to process this video. We combine the speed of cutting on keyframes + accurate cut with re-encoding only between 2 keyframes. So no matter if you remove a segment from a 10 minute or 10h video the time is the same.

## zncut

Accurate cuts. Valid file. Almost no re-encoding. I'm sharing this because I built it, it works, and I'm proud of it.

I've written it by hand as my first ever Python project. It's free and open sourced. Check out [zncut on Github](https://github.com/zincplusplus/zncut).
