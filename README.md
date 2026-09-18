# AI-Powered YouTube Video Automation with n8n

An end-to-end AI-powered YouTube video automation pipeline built with **n8n**. The system automates content ideation, script generation, AI image generation, image-to-video generation, voice generation, video/audio merging, and final YouTube upload.

---

## 🚀 Project Overview

This project demonstrates how multiple AI services and automation tools can be connected through **n8n** to create an automated YouTube video production pipeline.

The workflow is divided into two major stages:

### Stage 1 — AI Content & Media Generation

The first workflow generates the required content and media:

- Generates a video idea
- Creates a structured video script
- Splits the script into short video segments
- Generates image prompts for each segment
- Generates AI images
- Uploads generated images to Google Drive
- Sends images to Kling AI for image-to-video generation
- Generates voice-over using ElevenLabs
- Stores generated audio in Google Drive

### Stage 2 — Video Processing & YouTube Upload

The second workflow:

- Receives a webhook callback
- Checks Kling AI video generation status
- Downloads the generated video
- Merges the generated video with the ElevenLabs audio
- Retrieves the processed video
- Downloads the final video
- Uploads the finished video to YouTube

---

## 🏗️ Workflow Architecture

The complete automation follows this flow:

Manual Trigger
→ Ideator
→ Script Generation
→ Split Script into 6-Second Segments
→ Generate Image Prompts
→ Generate AI Images
→ Upload Images to Google Drive
→ Kling AI Image-to-Video
→ Video Processing
→ Merge Video + Audio
→ Final Video
→ YouTube Upload

A parallel voice-generation branch runs from the generated script:

Script
→ ElevenLabs Text-to-Speech
→ Upload Audio to Google Drive

---

## 🔹 Workflow 1 — AI Video Generation

**File:** `workflows/youtube-ai-video-generation.json`

This workflow is responsible for generating the content and media required for the final YouTube video.

### Workflow Flow

Manual Trigger
→ Ideator
→ Script Generation
→ Split Script into 6-Second Segments
→ Generate Image Prompts
→ Generate AI Images
→ Upload Images to Google Drive
→ Kling AI Image-to-Video

Parallel voice branch:

Script
→ ElevenLabs Text-to-Speech
→ Upload Audio to Google Drive

---

## 🧠 AI Content Generation

The **Ideator** generates the initial video concept.

The script generation step then produces structured content including:

- Intro
- Main content
- Call-to-action
- Title
- Description

The structured format allows the generated content to be processed automatically by later workflow steps.

---

## ✂️ Script Segmentation

The generated script is divided into short segments suitable for video generation.

The workflow uses approximately:

- **2.5 words per second**
- **6-second video segments**

This allows each section of the script to be connected with an individual visual scene.

---

## 🎨 AI Image Generation

For each script segment:

1. An AI model generates an image prompt.
2. The prompt is sent to the image generation model.
3. The AI image is generated.
4. The image is uploaded to Google Drive.
5. The image is provided to Kling AI for image-to-video generation.

---

## 🎬 Image-to-Video Generation

**Kling AI** converts the generated images into short video clips.

The workflow sends the generated image and required generation parameters to Kling AI.

Kling AI processes the request asynchronously and provides a task/status that can be monitored by the second workflow.

---

## 🎙️ AI Voice Generation

The generated script is also sent to **ElevenLabs** for text-to-speech generation.

The resulting voice-over audio is stored in Google Drive and later combined with the generated video.

---

## 🔹 Workflow 2 — Video Processing & YouTube Upload

**File:** `workflows/video-processing-youtube-upload.json`

This workflow handles the final video processing and publishing stage.

### Workflow Flow

Webhook
→ Check Kling AI Video Status
→ IF Status = Succeed
→ Download Generated Video
→ Merge Video + Audio
→ Check Processing Status
→ Retrieve Processed Video URL
→ Download Final Video
→ Upload to YouTube

---

## 🔗 Webhook Integration

The second workflow begins with a webhook endpoint.

The webhook is used to receive information related to the Kling AI video-generation process.

The workflow then checks whether the video generation was successful before continuing with the processing pipeline.

---

## 🎞️ Video & Audio Merging

The generated video and ElevenLabs voice-over are combined using **FFmpeg through fal.ai**.

This creates the final video containing:

- AI-generated visuals
- AI-generated voice-over

---

## 📤 YouTube Upload

After the final video has been processed and downloaded, the workflow automatically uploads the finished video to YouTube.

This removes the need to manually download and upload each generated video.

---

## 🛠️ Technologies & Tools

| Technology | Purpose |
|---|---|
| **n8n** | Workflow automation and orchestration |
| **OpenAI** | AI image prompt and image generation |
| **Grok** | AI ideation and content generation |
| **Kling AI** | Image-to-video generation |
| **ElevenLabs** | AI voice generation |
| **Google Drive** | Media storage |
| **fal.ai / FFmpeg** | Video and audio merging |
| **YouTube API** | Automated video publishing |
| **Webhooks** | Workflow communication and callbacks |
| **REST APIs** | Connecting external AI services |

---

## 📸 Workflow Screenshots

### AI Video Generation Workflow

![AI Video Generation Workflow](screenshots/01-ai-video-generation-workflow.png)

### Video Processing & YouTube Upload Workflow

![Video Processing and YouTube Upload](screenshots/02-video-processing-youtube-upload.png)

---

## 📂 Repository Structure

The repository is organized as follows:

youtube-ai-automation-n8n/
├── README.md
├── workflows/
│   ├── youtube-ai-video-generation.json
│   └── video-processing-youtube-upload.json
├── screenshots/
│   ├── 01-ai-video-generation-workflow.png
│   └── 02-video-processing-youtube-upload.png
└── docs/
    └── workflow-architecture.md

---

## ⚙️ Setup Overview

To recreate this automation, you will need access to the required services and APIs.

### 1. Set Up n8n

Set up an n8n instance and import the workflow JSON files from the `workflows` directory.

### 2. Configure AI Credentials

Configure the required credentials for:

- OpenAI
- Grok
- Kling AI
- ElevenLabs

### 3. Configure Google Drive

Create Google Drive credentials in n8n and configure the folders where generated images and audio will be stored.

### 4. Configure fal.ai

Configure your fal.ai API credentials for the FFmpeg video/audio merging operation.

### 5. Configure YouTube

Configure YouTube OAuth credentials in n8n so the workflow can upload the final video.

### 6. Configure the Webhook

Configure the Kling AI callback/webhook URL so the video-processing workflow can receive information about the video-generation process.

---

## 🔐 Security

**Important:** API keys and OAuth credentials must never be committed to a public GitHub repository.

Before publishing this project:

- Remove all API keys from workflow JSON files.
- Remove private credentials.
- Replace sensitive values with placeholders.
- Use n8n Credentials for authentication.
- Never commit `.env` files containing secrets.

Example placeholders:

- `YOUR_KLING_API_KEY`
- `YOUR_ELEVENLABS_API_KEY`
- `YOUR_FAL_AI_API_KEY`
- `YOUR_OPENAI_API_KEY`

The workflow files in this repository should contain only sanitized configuration and placeholder values.

---

## 💡 Automation Concepts Demonstrated

This project demonstrates practical experience with:

- AI workflow automation
- n8n workflow design
- AI content generation
- Prompt engineering
- Structured AI outputs
- Script processing
- Text-to-speech automation
- AI image generation
- Image-to-video generation
- REST API integration
- Webhook integration
- Asynchronous API processing
- Status checking
- Binary file handling
- Cloud storage automation
- FFmpeg-based media processing
- YouTube API automation
- Multi-step workflow orchestration

---

## 📈 End-to-End Automation

The complete automation can be summarized as:

Idea
→ AI Script
→ Script Segmentation
→ Image Prompts
→ AI Images
→ AI Video Clips
→ AI Voice
→ Video + Audio Merge
→ Final Video
→ YouTube

The objective is to reduce the manual work involved in producing and publishing AI-generated YouTube content by connecting multiple AI and automation services into a single pipeline.

---

## 🎯 Key Learning Outcomes

Through this project, I practiced:

- Designing multi-stage n8n workflows
- Connecting multiple external APIs
- Working with AI models inside automation workflows
- Handling asynchronous API responses
- Using webhooks for service callbacks
- Processing generated media
- Automating cloud storage
- Automating YouTube publishing
- Building an end-to-end AI automation pipeline

---

## 📌 Project Status

**Status:** Completed / Portfolio Project

The repository contains the workflow JSON files and screenshots demonstrating the automation architecture.

---

## 👤 Author

**Sabuj Chandra Das**

AI Automation Engineer

- LinkedIn: [Add LinkedIn URL]
- GitHub: [Add GitHub URL]
- Portfolio: [Add Portfolio URL]
- Email: [Add Email]

---

## ⭐ Project Highlights

This project combines multiple AI services and automation technologies into one end-to-end pipeline.

It demonstrates how **n8n can be used as an orchestration layer to connect AI models, media-generation APIs, cloud storage, media-processing services, webhooks, and YouTube publishing into an automated production workflow.**
