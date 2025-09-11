---
id: api-intro
title: API Introduction
sidebar_position: 1
---

Welcome to the Audixa AI API Reference! Our API is built on REST principles, making it easy to integrate high-quality text-to-speech generation directly into your applications. All requests accept JSON-formatted bodies and return JSON responses, using standard HTTP response codes.

This guide provides the essential information you need to get started, including authentication and the core asynchronous workflow.

---

## Base URL

All API endpoints are versioned and accessible from the following base URL:

```

https://api.audixa.ai/v2

````

---

## Authentication

Authentication is required for all endpoints and is handled by passing your private API key in the `x-api-key` HTTP header.

You can find and manage your API keys in the [API section of your dashboard](https://audixa.ai/dashboard/api). Remember to keep your keys secret and never expose them in client-side code.

All API requests should include the header as follows:

```http
x-api-key: YOUR_API_KEY
````

Any request made without a valid API key will return a `401 Unauthorized` error.

-----

## Core Workflow: Asynchronous Generation

To provide the fastest possible response times, our API operates **asynchronously**. Instead of waiting for the audio to be generated (which can take several seconds), the API immediately confirms your request and lets you retrieve the audio moments later.

This workflow applies to all TTS generation and involves two simple steps:

### Step 1: Submit the Generation Task

You send a `POST` request to the `/tts` endpoint with your text, voice ID, and other parameters. The API validates your request and credits, then queues the task.

If successful, the API immediately returns a `201 Created` response containing a unique **`generation_id`**.

```json
{
  "generation_id": "base_27b4695b-017f-4b6e-8121-03264624479c"
}
```

### Step 2: Retrieve the Audio

You then use this `generation_id` to poll the `/status` endpoint using a `GET` request.

  - While the audio is processing, the endpoint will return a response with `"status": "Generating"`.
  - Once completed, the response will update to `"status": "Completed"` and include the `audio_url` where you can download your MP3 file.
  - If the generation fails, the status will show `"Failed"`.

-----

## API Endpoint Overview

This reference guide is organized into the following main endpoints:

| Endpoint         | Method | Description                                                |
| :--------------- | :----- | :--------------------------------------------------------- |
| **`/tts`** | `POST` | Submits a new text-to-speech generation task.              |
| **`/status`** | `GET`  | Retrieves the status and audio URL of a generation task.   |
| **`/voices`** | `GET`  | Fetches a complete list of voices available to your account. |

-----

Ready to get started? Head over to the [**First API Request**](/getting-started/first-request) guide to generate your first audio file.
