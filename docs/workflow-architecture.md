# Workflow Architecture

## Overview

This project consists of two interconnected n8n workflows that automate the process of generating, processing, and publishing AI-powered YouTube videos.

The system separates media generation from final video processing and publishing.

---

# Workflow 1 — AI Video Generation

**File:** `youtube-ai-video-generation.json`

This workflow is responsible for generating the content, images, video clips, and voice-over required for the final video.

## Main Flow

Manual Trigger
→ Ideator
→ Script Generation
→ Script Segmentation
→ Image Prompt Generation
→ AI Image Generation
→ Google Drive
→ Kling AI Image-to-Video

## Voice Generation Branch

The generated script is also sent through a separate voice-generation branch:

Script
→ ElevenLabs Text-to-Speech
→ Google Drive

## Detailed Process

### 1. Ideation

The workflow starts with an AI ideation step that generates the concept for the YouTube video.

### 2. Script Generation

The generated idea is converted into a structured script containing:

- Intro
- Main content
- Call-to-action
- Title
- Description

### 3. Script Segmentation

The script is divided into short segments.

The workflow uses approximately 2.5 words per second and targets approximately 6 seconds per segment.

This allows each segment to correspond to a visual scene.

### 4. Image Prompt Generation

An AI model generates a visual prompt for each script segment.

These prompts are then used to generate the corresponding images.

### 5. AI Image Generation

The generated prompts are sent to an image-generation model.

Each script segment receives a corresponding AI-generated image.

### 6. Google Drive Storage

Generated images are uploaded to Google Drive so that they can be accessed by subsequent workflow steps.

### 7. Kling AI Image-to-Video

The generated images are sent to Kling AI.

Kling AI converts the static images into short video clips.

The video-generation process is asynchronous, so the workflow provides the required information for the second workflow to monitor the generation process.

---

# Workflow 2 — Video Processing & YouTube Upload

**File:** `video-processing-youtube-upload.json`

The second workflow is responsible for monitoring the generated video, processing the media, merging audio and video, and uploading the final result to YouTube.

## Main Flow

Webhook
→ Check Kling AI Status
→ Check Success Condition
→ Download Generated Video
→ Merge Video + Audio
→ Check Processing Status
→ Retrieve Processed Video
→ Download Final Video
→ YouTube Upload

## Detailed Process

### 1. Webhook

The workflow starts with a webhook endpoint.

The webhook is used as the entry point for information related to the Kling AI video-generation process.

### 2. Check Video Generation Status

The workflow checks the current status of the Kling AI generation task.

The workflow continues only when the required success condition is satisfied.

### 3. Download Generated Video

Once the generated video is available, the workflow retrieves and downloads the video file.

### 4. Merge Video and Audio

The generated video is combined with the voice-over audio produced by ElevenLabs.

The merging operation uses FFmpeg through fal.ai.

### 5. Check Processing Status

The workflow checks the status of the media-processing operation.

### 6. Retrieve Final Video

Once processing is complete, the workflow retrieves the URL of the processed video.

### 7. Download Final Video

The processed video is downloaded so it can be passed to the YouTube upload step.

### 8. Upload to YouTube

The final video is uploaded to YouTube through the configured YouTube integration.

---

# System Architecture

The overall system can be represented as:

AI Ideation
↓
Script Generation
↓
Script Segmentation
↓
Image Prompt Generation
↓
AI Image Generation
↓
Google Drive
↓
Kling AI
↓
Video Generation
↓
Webhook
↓
Video Status Check
↓
Generated Video
↓
FFmpeg / fal.ai
↓
Video + ElevenLabs Audio
↓
Final Video
↓
YouTube

---

# External Services

## n8n

Used as the central automation and orchestration platform.

## OpenAI

Used for AI-powered content/image prompt generation and image generation within the workflow.

## Grok

Used for AI ideation and content generation.

## Kling AI

Used to transform generated images into video clips.

## ElevenLabs

Used to generate AI voice-over audio from the video script.

## Google Drive

Used for storing generated images and audio files.

## fal.ai / FFmpeg

Used to merge the generated video and voice-over audio.

## YouTube

Used as the final publishing platform.

---

# Workflow Communication

The two workflows are designed as separate stages.

The first workflow handles AI content and media generation.

The second workflow handles video processing and publishing.

The separation allows the system to handle asynchronous video generation and processing operations without keeping the entire pipeline inside a single workflow.

---

# Security Considerations

API keys and authentication credentials should never be stored directly in a public GitHub repository.

Production credentials should be configured through n8n's credential system or secure environment configuration.

Any workflow JSON committed to this repository should be sanitized and must not contain active API keys, OAuth tokens, or other private credentials.
