+++
date = '2025-07-12T17:10:27+08:00'
draft = false
title = 'My Smart Assistant 3.0: Building a Fully Automated "Time Screening Room"'
+++

Welcome back to my "lazy" photography system series. In the previous chapters, we witnessed how an ordinary webcam was endowed with the ability to "think," and how I built an elegant and convenient "control room" for it. However, the story doesn't end there. Our clever photographer has been smartly "slacking off" every day, accurately capturing a vast number of valuable moments. These photos, like scattered pearls, contain the secrets of time, quietly resting deep within the server's hard drive.

It's time to string these pearls together, letting them shine brilliantly through the flow of light and shadow. Today, I will show you the final chapter of this project, which I personally find the most exciting — building a fully automated "Time Screening Room." At the core of this screening room is a powerful and reliable shell script. It is the "behind-the-scenes director" of my entire system, entrusted with a sacred mission: before each quiet dawn arrives, automatically merging tens of thousands of photos from the previous day into wonderful time-lapse videos.

Project code: https://github.com/AndroidOL/camera-capture/tree/main/merge

## From "Material Pileup" to "Visual Poems": The Final "Billion" Step

Imagine what life would be like without this automated "director." It would mean that every day, I’d have to act like a "miner" in the digital age — manually logging into a remote server to dig through tens of thousands of photos from the previous day, which might amount to dozens of gigabytes; then carefully downloading these "raw ores" to my local computer; next, launching complex video editing software, painstakingly dragging each photo onto the timeline, setting frame rates, encoders, bitrates... and finally clicking "render" after a long wait.

This entire process is not only a battle of patience and endurance with no creativity but also prone to errors. If one day I forget or get too busy at work and miss it, the "visual diary" for that day will have an irreplaceable gap. That's not what I want.

What I need is a fully unattended, extremely stable and reliable, and smart enough automated process. It should be a tireless artist that quietly starts at a fixed time every day, for example, in the dead of night, and like a skilled film editor, carefully arranges the scattered materials of the previous day into a complete film, then elegantly places it on the digital shelf of the "family film library," waiting for my review.

Thus, this labor of love — the automation script `daily_photo_to_video.sh` — was born. It’s far from a simple ffmpeg command wrapper; it is an industrial-grade robust "smart production line."

## The Director's Extraordinary Skills: Revealing the Core Design of the Automation Script

The design philosophy of this script is **"pursue ultimate stability and efficiency."** I armed it to the teeth so that it can calmly handle all foreseeable and even unforeseeable complex situations.

### 1. Ultimate Efficiency: Squeezing Every Drop of Hardware Potential

**Highway — Ramdisk (In-Memory Disk)**  
Reading tens of thousands of scattered small images on a traditional hard drive while writing a huge video file immediately exposes disk I/O bottlenecks, making the whole process painfully slow. My solution is radical: directly carve out a high-speed virtual disk (Ramdisk) in the server’s precious memory.

When the script starts, it first synchronizes all photos to be processed from the photo server to this memory disk via high-speed network. Then, all intensive read-write operations of video synthesis (encoding) are performed entirely on this virtual disk. Memory read/write speeds are tens or even hundreds of times faster than ordinary hard drives, which is like building a "star highway" that ignores physical laws and travels at light speed for data migration, greatly shortening processing time.

**Professional Engine — Hardware Encoding (HEVC_RKMPP)**  
Video encoding is a typical CPU-intensive task. Relying solely on the CPU to "hard compute" is like asking a versatile but tired butler to do a highly specialized woodworking job — inefficient and causing the server’s "heartbeat" (CPU usage) to spike and the fans to roar. Fortunately, the server I use (based on Rockchip chips) has a dedicated video processing unit (VPU).

My script precisely calls ffmpeg and specifies the use of the `hevc_rkmpp` hardware encoder. This is equivalent to handing the professional work to a specially hired, highly skilled "film editor," letting the VPU handle the heaviest encoding tasks, fully freeing the CPU. Meanwhile, I choose the efficient HEVC (H.265) encoding format, which maintains very high picture quality while compressing the video size to about half of traditional H.264, saving massive precious space in my "family film library."

### 2. Foundation of Stability: Industrial-Grade Fault Tolerance and Self-Healing

A spaceship sailing alone in the wilderness values not only speed but also its ability to survive cosmic storms. Likewise, a script that needs to run unattended for years requires strong resilience. I built four layers of "security protocols" to handle various possible accidents.

| Scenario          | Script’s Smart Response Strategy                          |
|-------------------|-----------------------------------------------------------|
| Unexpected script interruption | Uses `trap` cleanup mechanism to automatically clear temp files and unmount Ramdisk on interruption |
| Concurrent run conflicts      | Uses `flock` file lock to avoid conflicts from multiple instances running concurrently          |
| Insufficient storage space    | Pre-checks available space in target path to avoid write failures midway                        |
| Target disk disconnection     | Automatically switches to backup storage path, ensures task completion, and pauses old task catch-up |

### 3. Smart Scheduling: Never Miss a Day’s Highlights

I want every day’s video in my "screening room" to be complete, like a coherent chronicle. Therefore, the smartest and proudest part of the script is its almost obsessive "completion" logic.

When running, the script:

- Reviews logs from the past 7 days, checking for missing or failed videos;
- If omissions are found, it prioritizes filling gaps starting from the earliest day;
- Verifies that remote materials still exist to avoid pointless runs;
- Only after confirming completeness does it process "yesterday’s" new materials.

This way, the system gains memory and responsibility, always maintaining a complete "visual diary."

## The Birth of a "Family Epic"

Now, this has become a ritual-filled daily routine. Every morning, when I brew coffee and open the "family film library," a brand-new time-lapse video named after yesterday’s date quietly rests there.

It might capture the entire thunderstorm outside the window, or my figure working late into the night, or simply portray the traces of sunlight flowing through the home.

This automated script is like a silent but powerful heart, injecting endless vitality into my entire smart photography project. It transforms day-after-day trivial visual data into distilled visual poems full of time’s essence, turning cold data into warm memories, allowing me to truly "see" and "cherish" those silently passing moments.

## The Joy of Creation

The joy of creation lies here — using code and logic to build a system that automatically creates, organizes, and presents beauty for us.

Next steps might be training an AI to automatically add the most fitting background music to these videos, or editing "weekly highlights."

The journey to a "better life" through creation is endless, and that is its true charm.
