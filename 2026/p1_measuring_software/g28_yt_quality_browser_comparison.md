---
author: Uddhav Pisharody, Job Stouthart, Vasil Chirov, Georgi Dimitrov, Horia Zaharia
group_number: 28
title: "Energy Consumption of YouTube Short-Form Video Playback: Auto vs HD720 on Chrome and Firefox"
image: "img/gX_template/project_cover.png"
date: 27/02/2026
summary: |-
  We investigate the energy consumption of short-form video playback on YouTube under controlled conditions. Specifically, we analyze how browser choice (Google Chrome vs Mozilla Firefox) and video quality settings (automatic quality selection vs forced 720p playback) affect energy usage. Using the same YouTube Short video across all experiments, we construct a 2×2 experimental design consisting of Chrome–Auto, Chrome–HD720, Firefox–Auto, and Firefox–HD720 configurations. Energy consumption is measured using EnergiBridge at the CPU package level while automating video playback via Playwright. Each configuration is executed 30 times in randomized order to mitigate system noise. Our goal is to assess whether differences in video quality and browser implementations lead to measurable differences in energy consumption and efficiency during short-form video playback.
identifier: p1_measuring_software_2026 # Do not change this
all_projects_page: "../p1_measuring_software" # Do not change this
---

## Introduction
Video streaming has completely changed how we spend our time online. However, when it comes to streaming, YouTube is still the number one most used website[^1]. According to YouTube's CEO[^2], people have watched over 1 billion hours of YouTube on their TVs every day. Because of this, the platform's energy use really adds up.

In this study, we measured the energy and power consumption of streaming YouTube videos under four conditions:
1. **Google Chrome** on Auto Quality
2. **Google Chrome** on 720p HD Quality
3. **Mozilla Firefox** on Auto Quality
4. **Mozilla Firefox** on 720p HD Quality

The goal of this report is to determine how video quality and browser choice affect energy consumption.

## Background

According to Cloudflare[^3], streaming is the continuous transmission of audio or video files from a server to a client device. Unlike downloading, where you have to save the whole file to your hard drive before you can open it, streaming lets you start watching almost immediately.

The process works by breaking the video data into small packets. These packets are sent over the internet and later organized by your browser so the video plays continuously and smoothly. Most streaming services use a buffer to load the next few seconds of the video before they are even played. This helps prevent the video from freezing if your internet speed drops for a moment. To make this faster, companies often use Content Delivery Networks (CDNs) to store the video on servers physically closer to the user, reducing the distance the data has to travel.

YouTube takes streaming a step further with its auto-quality setting. The auto-quality setting allows for adaptive bitrate streaming. With this setting, your browser uses data to pick which set of bits to send. If your internet speed is high, it will send higher-quality packets. In comparison, if it dips or is generally slower, it will pick lower-quality packets, so the video does not buffer[^4].

The choice of browser is also very important; Chrome and Firefox might look similar on the outside, but they use completely different engines. A browser engine consists of 2 main engines. The rendering engine, which visualizes the data for you, and the JavaScript engine, which compiles and executes the code[^5]. Chrome uses the Blink rendering engine created by themself together with the V8 JavaScript engine, while Firefox uses Gecko and SpiderMonkey for these operations[^5].


## Methodology

To ensure accurate and reproducible results, we automated our testing pipeline using a custom Bash script. All experiments were conducted on a Linux machine with an **[PLEASE ADD PROCESSOR AND PC INFORMATION HERE]**. 

We used **Energibridge** to measure the raw energy and power consumption during the video playback, exporting the telemetry data into CSV files for analysis. Our automated pipeline read from a pre-generated execution plan and used a Python script (`play_video.py`) to launch the specified browser, navigate to the YouTube URL, and set the target video quality.

**CONTINUE METHODOLOGY** Talk about ZEN and how we changed settings so the screen doesn't change, etc.

## Results

The most important thing we found is that browser choice matters much more than video quality. Even when lowering the resolution, using the wrong browser can still waste significant energy.

### Energy Comparison
The tests showed that Firefox is much more efficient than Chrome. 

<img src="img/g28_yt_quality_browser_comparison/energy_violin.png" alt="Energy Consumption Comparison" width="1500"/>

As shown in the chart above, Chrome's energy use is very tightly packed. 
* **Chrome** consistently uses about **360 Joules** on Auto and **375 Joules** on 720p. 
* **Firefox** is highly unpredictable. Its energy use swings wildly from as low as **280 Joules** to over **415 Joules**.

While switching to 720p HD, both browsers used more power, but the difference was small. While Firefox was highly unpredictable.

---

### Steady Power vs. Spikes

By looking at power usage over time, we found out why Firefox uses more power. 

<img src="img/g28_yt_quality_browser_comparison/power_over_time.png" alt="Power Time Series" width="1500"/>

Right when a video starts, Chrome has huge power spikes. It jumps above *15 Watts** in the first 10 seconds. Firefox is much smoother and rarely goes above **15 Watts**. However Firefox remains around this power level while chrome loads everything with a burst and than reduces to lower levels of power consumption afterwards.

---

### Is Speed Worth the Extra Energy?
We compared how much energy was used versus how long the video task took to finish.

<img src="img/g28_yt_quality_browser_comparison/energy_vs_duration.png" alt="Time and Efficiency" width="1500"/>

This scatter plot shows exactly why Chrome is better. The blue and orange dots for Chrome are tightly grouped together. Chrome finishes the video loading task in a very consistent **32 to 33 seconds**. 
The green and red dots for Firefox are scattered all over the place. Firefox can take anywhere from **30 seconds to 38 seconds** to finish the same exact task
### Sources
[^1]: [Cloudflare Radar 2025 Year in Review](https://radar.cloudflare.com/year-in-review/2025)
[^2]: [YouTube CEO 2025 Priorities: Our Big Bets](https://blog.youtube/inside-youtube/our-big-bets-for-2025/)
[^3]: [What is streaming?](https://www.cloudflare.com/learning/video/what-is-streaming/)
[^4]: [What is adaptive bitrate streaming?](https://www.cloudflare.com/learning/video/what-is-adaptive-bitrate-streaming/)
[^5]: [Browser Engines: The Crux of Cross Browser Compatibility](https://www.testmuai.com/blog/browser-engines-the-crux-of-cross-browser-compatibility/)