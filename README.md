yt2clip.live
A modern, fast, and intuitive web application to clip, cut, and convert any non-DRM YouTube video directly from your browser.







Live Demo: https://www.yt2clip.live
yt2clip.live is a full-stack web application designed to provide a seamless experience for creating video clips or extracting audio from YouTube videos. It leverages a powerful, asynchronous backend to process video streams on-the-fly without needing to download the entire video first.

✨ Key Features
On-the-Fly Processing: Utilizes a stream-based approach with ffmpeg, ensuring fast processing times even for long videos.
Dual Mode Functionality:
Clip Video: Cut precise segments from videos with an interactive timeline.
Download Audio: Convert and download the full audio track of any video.
Interactive UI: A sleek, responsive interface built with Next.js and Tailwind CSS, featuring a draggable timeline slider for millisecond-precision.
Multiple Formats: Supports various output formats including MP4, MOV for video and MP3, WAV, OGG for audio.
Real-time Feedback: Employs WebSockets to provide live updates on job progress, from queueing to completion.
Secure & Robust: Built with modern security best practices, including rate limiting, input validation, secure headers (CSP), and an automated cookie renewal system.
Analytics & Logging: Tracks anonymous usage data for feature analysis and logs all processing jobs to a PostgreSQL database for monitoring.
🛠️ How It Works (Architecture)
The application is built on a decoupled microservices architecture to handle requests asynchronously and efficiently.

The Next.js Frontend, hosted on Vercel, sends a job request to the Spring Boot API.
The Spring Boot API acts as the orchestrator. It validates the request, manages the user's WebSocket session, and places a job onto a Redis queue.
The Python Worker (running in a Docker container) is a powerful processing engine. It picks up the job from the queue, uses yt-dlp to fetch the video stream URL (with auto-renewed cookies via Selenium), and processes it with ffmpeg.
Upon completion (or failure), the worker logs the outcome to a PostgreSQL database (hosted on Neon.tech) and publishes a result message to a Redis Pub/Sub channel.
The Spring Boot API, listening to the channel, receives the result and pushes a real-time notification to the correct user's browser via WebSockets.
The entire infrastructure is protected and accelerated by Cloudflare, which provides DNS, SSL, and security services.
🚀 Tech Stack
Frontend: Next.js, React, TypeScript, Tailwind CSS, Vercel Analytics
Backend API: Java, Spring Boot, Spring Security, Spring Web, WebSockets
Backend Worker: Python, Selenium, yt-dlp, ffmpeg-python, psycopg2
Infrastructure & DevOps: Docker, Docker Compose, Redis, PostgreSQL (Neon.tech), Nginx (as Reverse Proxy), Cloudflare, DigitalOcean, Vercel
🎬 Usage
Visit https://www.yt2clip.live.
Paste a non-DRM protected YouTube URL into the input field.
Wait for the video information to load.
Select your desired mode: 'Clip Video' or 'Download Audio'.
Adjust the settings (format, quality, and time range for clips).
Click 'Create' and watch the real-time status updates.
Download your file once the job is complete!
