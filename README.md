# Spacely AI Business API

A unified API suite for generating creative renders, performing style transfer, and auto-furnishing spaces using artificial intelligence. Easily transform photos of residential spaces and retrieve design outcomes programmatically.


- [Spacely AI Creative Render API](#spacely-ai-creative-render-api)
- [Spacely AI Style Transfer API](#spacely-ai-style-transfer-api)
- [Spacely AI Auto Furnish API](#spacely-ai-auto-furnish-api)
- [Spacely AI Empty the Room](#spacely-ai-empty-the-room)
- [Spacely AI Magic Edit API](#spacely-ai-magic-edit)
- [Asynchronous Mode](#using-async-mode)
- [Viewing All Presets & Configurations](#viewing-all-presets--configurations)
- [Support](#support)


<br><br>


# Spacely AI Creative Render API 

This document describes how to use the **Spacely AI Creative Render API** for generating creative renders from input images using different styles and options. This API is intended for rendering residential spaces, such as living rooms, in various styles.

---

## Endpoint

```
POST https://business-api.spacely.ai/api/generations/creative-render
```

---

## Authentication

- **API Key:** Every request must include an `X-API-KEY` header.
- **Example:**
  ```
  X-API-KEY: YOUR_API_KEY
  ```

---

## Request Headers

| Header           | Value                   | Required | Description            |
|------------------|------------------------|----------|------------------------|
| X-API-KEY        | YOUR_API_KEY           | Yes      | Your API access key    |
| Content-Type     | application/json        | Yes      | Data content type      |

---

## Request Body

Send a JSON object with the following structure:

| Field            | Type       | Required | Description                                                        |
|------------------|------------|----------|--------------------------------------------------------------------|
| mode             | string     | Yes      | Generation mode. e.g. `sync`, `async`                              |
| type             | string     | Yes      | Space type. e.g. `residential`, `commercial`, `exterior`, `event`      |
| inputImage       | string     | Yes      | URL to the input image                                             |
| space            | string     | Yes      | Space name (e.g. `living_room`, `bedroom`, etc.)  [see all presets here](#viewing-all-presets--configurations)                |
| style            | string     | Yes      | Desired style (e.g. `modern`, `classic`, etc.)    [see all presets here](#viewing-all-presets--configurations)                |
| settings         | object     | Yes      | Generation settings, see below                                     |
| spacePrompt      | string     | No       | Additional prompt for the space                                    |
| stypePrompt      | string     | No       | Additional prompt for the style                                    |
| options          | object     | No       | Additional options, see below                                      |

### Example

```json
{
  "mode": "sync",
  "type": "residential",
  "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/6ecec56c-7373-4ba1-8b2e-04c162d4bd96.png",
  "space": "living_room",
  "style": "modern",
  "spacePrompt": "",
  "stypePrompt": "",
  "options": {
    "lighting": "",
    "color": ""
  },
  "settings": {
    "creativityLevel": "precise"
  }
}
```

#### `options` object

| Field    | Type   | Example | Description                        |
|----------|--------|---------|------------------------------------|
| lighting | string | "daylight"  | Lighting preference (optional)  [see all presets here](#viewing-all-presets--configurations)   |
| color    | string | "palette_1"  | Color preference (optional)   [see all presets here](#viewing-all-presets--configurations)      |

#### `settings` object

| Field           | Type   | Example   | Description                          |
|-----------------|--------|-----------|--------------------------------------|
| creativityLevel | string | "precise" | Can be `precise`, `balanced`, `creative`, etc.   |

---


## Example cURL Request

```bash
curl --location 'https://business-api.spacely.ai/api/generations/creative-render' \
  --header 'X-API-KEY: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "mode": "sync",
    "type": "residential",
    "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/6ecec56c-7373-4ba1-8b2e-04c162d4bd96.png",
    "space": "living_room",
    "style": "modern",
    "spacePrompt": "",
    "stypePrompt": "",
    "options": {
      "lighting": "",
      "color": ""
    },
    "settings": {
      "creativityLevel": "precise"
    }
  }'
```

---

## Example Response

```json
{
  "generationId": "ccc54abf-be94-42c8-a49f-8aefcfebfe94",
  "status": "succeeded",
  "input": {
    "mode": "sync",
    "type": "residential",
    "space": "living_room",
    "style": "modern",
    "options": {
      "color": "",
      "lighting": ""
    },
    "settings": {
      "creativityLevel": "precise"
    },
    "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/e271a958-707f-447f-9647-85ca13fba674.png",
    "spacePrompt": "",
    "stypePrompt": ""
  },
  "outputs": [
    "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/c871c05a-0a41-4e94-be1a-402a213065bd.png",
    "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/af194981-23ed-4ed0-85fc-0899ae9ec47c.png",
    "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/10ebf55f-aa25-4e06-97dc-62599c3d2d4e.png",
    "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/3d8e943a-eae2-4d35-9a6a-591e8ec02aab.png"
  ],
  "message": "succeeded",
  "percentage": 100
}
```

---

## Notes

- The input image must be publicly accessible via URL.
- Make sure your API key is kept private.
- Usage limits and rate limiting may apply.
- The response structure and output image URL pattern may change; check for updates in the API documentation.

---

## Error Responses

| HTTP Status | Description                 |
|-------------|-----------------------------|
| 400         | Bad Request                 |
| 401         | Unauthorized                |
| 500         | Internal Server Error        |

### Example Error Response

```json
{
  "error": {
    "code": "bad_request",
    "message": [
      "mode must be one of the following values: sync, async"
    ]
  }
}
```


<br><br>

# Spacely AI Style Transfer API

This document describes how to use the **Spacely AI Style Transfer API** to generate stylized renders from your input images using a variety of design styles and configurable options. The API is designed for style transfer tasks, allowing you to transform spaces such as living rooms, bedrooms, and more, into different visual aesthetics.

---

## Endpoint

```
POST https://business-api.spacely.ai/api/generations/style-transfer
```

---

## Authentication

- **API Key:** Every request must include an `X-API-KEY` header.
- **Example:**
  ```
  X-API-KEY: YOUR_API_KEY
  ```

---

## Request Headers

| Header           | Value                   | Required | Description            |
|------------------|------------------------|----------|------------------------|
| X-API-KEY        | YOUR_API_KEY           | Yes      | Your API access key    |
| Content-Type     | application/json        | Yes      | Data content type      |

---

## Request Body

Send a JSON object with the following structure:

| Field            | Type       | Required | Description                                                        |
|------------------|------------|----------|--------------------------------------------------------------------|
| mode             | string     | Yes      | Generation mode. e.g. `sync`, `async`                              |
| inputImage       | string     | Yes      | URL to the input image                                             |
| styleReference   | string     | Yes      | URL to the style reference image                                   |

> **Note:** When using `styleReference`, you do not need to provide the fields `type`, `space`, `style`, `spacePrompt`, or `stypePrompt`.

### Example

```json
{
  "mode": "async",
  "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/6ecec56c-7373-4ba1-8b2e-04c162d4bd96.png",
  "styleReference": "https://storage.googleapis.com/spacely/style-presets/scandinavian.png"
}
```

---

## Example cURL Request

```bash
curl --location 'https://business-api.spacely.ai/api/generations/style-transfer' \
  --header 'X-API-KEY: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "mode": "async",
    "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/6ecec56c-7373-4ba1-8b2e-04c162d4bd96.png",
    "styleReference": "https://storage.googleapis.com/spacely/style-presets/scandinavian.png"
  }'
```

---

## Example Response

```json
{
    "generationId": "c9bfe84c-d41e-43cf-a44f-67af3751e0e8",
    "status": "succeeded",
    "input": {
        "mode": "sync",
        "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/8d779d71-a2af-46dc-b4c1-ee1052ac25ed.png",
        "styleReference": "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/8af083c8-e1ba-4568-91f2-b8f00eb3d103.png"
    },
    "outputs": [
        "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/3fc87472-394e-46d5-a63a-5d5de61a0742.png",
        "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/3ff983e8-fe28-4fa9-aea0-118a70a4a085.png"
    ],
    "message": "succeeded",
    "percentage": 100
}
```

---

## Notes

- The input image must be publicly accessible via URL.
- Make sure your API key is kept private.
- Usage limits and rate limiting may apply.
- The response structure and output image URL pattern may change; check for updates in the API documentation.

---

## Error Responses

| HTTP Status | Description                 |
|-------------|-----------------------------|
| 400         | Bad Request                 |
| 401         | Unauthorized                |
| 500         | Internal Server Error        |

### Example Error Response

```json
{
  "error": {
    "code": "bad_request",
    "message": [
      "mode must be one of the following values: sync, async"
    ]
  }
}
```

<br><br>

# Spacely AI Auto Furnish API

This document explains how to use the **Spacely AI Auto Furnish API** to automatically generate fully furnished room renders from images of empty or unfurnished spaces. The API is designed to detect the room layout and add appropriate furniture and decor based on built-in design options, allowing you to quickly visualize how an empty living room, bedroom, or other space could look when furnished.

---

## Endpoint

```
POST https://business-api.spacely.ai/api/generations/auto-furnish
```

---

## Authentication

- **API Key:** Every request must include an `X-API-KEY` header.
- **Example:**
  ```
  X-API-KEY: YOUR_API_KEY
  ```

---

## Request Headers

| Header           | Value                   | Required | Description            |
|------------------|------------------------|----------|------------------------|
| X-API-KEY        | YOUR_API_KEY           | Yes      | Your API access key    |
| Content-Type     | application/json        | Yes      | Data content type      |

---

## Request Body

Send a JSON object with the following structure:

| Field        | Type   | Required | Description                                               |
|--------------|--------|----------|-----------------------------------------------------------|
| mode         | string | Yes      | Generation mode. e.g. `sync`, `async`                     |
| inputImage   | string | Yes      | URL to the input image                                    |
| space        | string | Yes      | Space name (e.g. `living_room`, `bedroom`, etc.)    [see all presets here](#viewing-all-presets--configurations)      |
| style        | string | Yes      | Desired style (e.g. `modern`, `classic`, etc.)    [see all presets here](#viewing-all-presets--configurations)        |
| spacePrompt  | string | No       | Additional prompt for the space (optional)                 |
| stylePrompt  | string | No       | Additional prompt for the style (optional)                 |

### Example

```json
{
  "mode": "sync",
  "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/fd1ac056-6e0e-4de2-84ee-0f72f087360a.png",
  "space": "living_room",
  "style": "modern",
  "spacePrompt": "",
  "stylePrompt": ""
}
```

---

## Example cURL Request

```bash
curl --location 'https://business-api.spacely.ai/api/generations/auto-furnish' \
  --header 'x-api-key: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "mode": "sync",
    "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/fd1ac056-6e0e-4de2-84ee-0f72f087360a.png",
    "space": "living_room",
    "style": "modern",
    "spacePrompt": "",
    "stylePrompt": ""
  }'
```

---

## Example Response

```json
{
    "generationId": "c9bfe84c-d41e-43cf-a44f-67af3751e0e8",
    "status": "succeeded",
    "input": {
        "mode": "sync",
        "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/8d779d71-a2af-46dc-b4c1-ee1052ac25ed.png",
        "styleReference": "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/8af083c8-e1ba-4568-91f2-b8f00eb3d103.png"
    },
    "outputs": [
        "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/3fc87472-394e-46d5-a63a-5d5de61a0742.png",
        "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/3ff983e8-fe28-4fa9-aea0-118a70a4a085.png"
    ],
    "message": "succeeded",
    "percentage": 100
}
```

---

## Notes

- The input image must be publicly accessible via URL.
- Make sure your API key is kept private.
- Usage limits and rate limiting may apply.
- The response structure and output image URL pattern may change; check for updates in the API documentation.

---

## Error Responses

| HTTP Status | Description                 |
|-------------|-----------------------------|
| 400         | Bad Request                 |
| 401         | Unauthorized                |
| 500         | Internal Server Error        |

### Example Error Response

```json
{
  "error": {
    "code": "bad_request",
    "message": [
      "mode must be one of the following values: sync, async"
    ]
  }
}
```

<br><br>

# Spacely AI Empty the Room

This guide explains how to use the **Spacely AI Empty the Room** API endpoint to automatically remove all furniture and decor from an input photo of a space. The API analyzes the submitted image, detects all visible furniture and decorative objects, and generates a new render showing the room as if it were completely empty. This feature is ideal for visualizing a bare space or for preparing photos for virtual staging.

---

## Endpoint

```
POST https://business-api.spacely.ai/api/generations/clear-room
```

---

## Authentication

- **API Key:** Every request must include an `X-API-KEY` header.
- **Example:**
  ```
  X-API-KEY: YOUR_API_KEY
  ```

---

## Request Headers

| Header           | Value           | Required | Description          |
|------------------|----------------|----------|----------------------|
| X-API-KEY        | YOUR_API_KEY   | Yes      | Your API access key  |
| Content-Type     | application/json| Yes     | Data content type    |

---

## Request Body

Send a JSON object using the following structure:

| Field       | Type   | Required | Description                   |
|-------------|--------|----------|-------------------------------|
| mode        | string | Yes      | Generation mode: `sync` or `async` |
| inputImage  | string | Yes      | Publicly accessible URL to the input image |

### Example

```json
{
  "mode": "sync",
  "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/fd1ac056-6e0e-4de2-84ee-0f72f087360a.png"
}
```

---

## Example cURL Request

```bash
curl --location 'https://business-api.spacely.ai/api/generations/clear-room' \
  --header 'x-api-key: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "mode": "sync",
    "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/fd1ac056-6e0e-4de2-84ee-0f72f087360a.png"
  }'
```

---

## Example Response

```json
{
    "generationId": "c9bfe84c-d41e-43cf-a44f-67af3751e0e8",
    "status": "succeeded",
    "input": {
        "mode": "sync",
        "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/8d779d71-a2af-46dc-b4c1-ee1052ac25ed.png",
        "styleReference": "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/8af083c8-e1ba-4568-91f2-b8f00eb3d103.png"
    },
    "outputs": [
        "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/3fc87472-394e-46d5-a63a-5d5de61a0742.png",
        "https://storage.googleapis.com/spacely-public-assets/user-assets/D76DYaqsaFT4YvUrbSZxuJWa6Y13/3ff983e8-fe28-4fa9-aea0-118a70a4a085.png"
    ],
    "message": "succeeded",
    "percentage": 100
}
```

---

## Notes

- The input image must be publicly accessible via URL.
- Make sure your API key is kept private.
- Usage limits and rate limiting may apply.
- The response structure and output image URL pattern may change; check for updates in the API documentation.

---

## Error Responses

| HTTP Status | Description                 |
|-------------|-----------------------------|
| 400         | Bad Request                 |
| 401         | Unauthorized                |
| 500         | Internal Server Error        |

### Example Error Response

```json
{
  "error": {
    "code": "bad_request",
    "message": [
      "mode must be one of the following values: sync, async"
    ]
  }
}
```

<br><br>

# Spacely AI Magic Edit

This guide explains how to use the **Spacely AI Magic Edit** API endpoint to make targeted changes to objects and content in your room photos using natural language prompts. The API analyzes the image and applies the edits described in your prompt, such as adding, removing, or modifying furniture or decor. Use this feature for flexible, on-demand creative editing.

---

## Endpoint

```
POST https://business-api.spacely.ai/api/generations/magic-edit
```

---

## Authentication

- **API Key:** Every request must include an `X-API-KEY` header.
- **Example:**
  ```
  X-API-KEY: YOUR_API_KEY
  ```

---

## Request Headers

| Header           | Value                   | Required | Description           |
|------------------|------------------------|----------|-----------------------|
| X-API-KEY        | YOUR_API_KEY           | Yes      | Your API access key   |
| Content-Type     | application/json        | Yes      | Data content type     |

---

## Request Body

Send a JSON object using the following structure:

| Field       | Type   | Required | Description                                                          |
|-------------|--------|----------|----------------------------------------------------------------------|
| mode        | string | Yes      | Generation mode: `sync` or `async`                                   |
| inputImage  | string | Yes      | Publicly accessible URL to the input image                           |
| prompt      | string | Yes      | Description of the edit you want (e.g., "add a TV", "remove the rug")|

### Example

```json
{
  "mode": "sync",
  "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/fd1ac056-6e0e-4de2-84ee-0f72f087360a.png",
  "prompt": "add tv"
}
```

---

## Example cURL Request

```bash
curl --location 'https://business-api.spacely.ai/api/generations/magic-edit' \
  --header 'x-api-key: YOUR_API_KEY' \
  --header 'Content-Type: application/json' \
  --data '{
    "mode": "sync",
    "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/fd1ac056-6e0e-4de2-84ee-0f72f087360a.png",
    "prompt": "add tv"
  }'
```

---

## Example Response

```json
{
    "generationId": "f2c3a198-5678-4dbc-805c-bf2e4fd6096e",
    "status": "succeeded",
    "input": {
        "mode": "sync",
        "inputImage": "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/fd1ac056-6e0e-4de2-84ee-0f72f087360a.png",
        "prompt": "add tv"
    },
    "outputs": [
        "https://storage.googleapis.com/spacely-public-assets/user-assets/3VuWYUUDHsQPx6ZILVFXqMfNofe2/edited-output-1.png"
    ],
    "message": "succeeded",
    "percentage": 100
}
```

---

## Notes

- The input image must be publicly accessible via URL.
- Make sure your API key is kept private.
- Usage and rate limits may apply.
- The response structure and output image URL pattern may change; check for updates in the API documentation.

---

## Error Responses

| HTTP Status | Description                  |
|-------------|------------------------------|
| 400         | Bad Request                  |
| 401         | Unauthorized                 |
| 500         | Internal Server Error        |

### Example Error Response

```json
{
  "error": {
    "code": "bad_request",
    "message": [
      "mode must be one of the following values: sync, async"
    ]
  }
}
```



## Using Async mode

If you use `"mode": "async"` in your request body, the API will return immediately with a `generationId` and status. The actual output images will not be available in this initial response.

To fetch the final result and outputs, you typically make a **follow-up GET request** using the returned `generationId`.  
Replace `<GENERATION_ID>` with the value you receive in your response.

Example cURL command to fetch the result:

```bash
curl --location 'https://business-api.spacely.ai/api/generations/<GENERATION_ID>' \
  --header 'x-api-key: YOUR_API_KEY'
```

Example with real value:

```bash
curl --location 'https://business-api.spacely.ai/api/generations/c9bfe84c-d41e-43cf-a44f-67af3751e0e8' \
  --header 'x-api-key: YOUR_API_KEY'
```

### Using Webhooks for Async Results

Alternatively, you can automate result retrieval by including a `webhookUrl` in your async request body.  
When you provide `"webhookUrl": "YOUR_WEBHOOK_URL"` (where `YOUR_WEBHOOK_URL` is the URL on your server that can receive POST requests), the API will send a POST request with the final result to your webhook once processing is finished. This way, you don't need to poll the GET endpoint repeatedly.

#### Example Request Body with Webhook

```json
{
  "mode": "async",
  // other fields...
  "webhookUrl": "https://your-server.com/my-callback"
}
```

When processing completes, your server will receive the final result payload at your webhook URL.

> **Note:** Make sure your webhook endpoint is publicly accessible and able to handle POST requests with JSON data.

Continue to poll this endpoint until the `status` in the response is `"succeeded"`, then use the `outputs` array for your generated images.




## Viewing All Presets & Configurations

You can explore all available presets and configuration options supported by the Spacely AI Business API by visiting the following link:

[View Business API Presets & Configs (JSON)](https://storage.googleapis.com/spacely-public-assets/presets/business-api-presets-latest.json)

This JSON file includes up-to-date lists of:
- Supported styles
- Spaces (e.g., living_room, bedroom, etc.)
- Preset configurations for different space types
- Available options and their values

Use this as a reference to see which styles, spaces, and presets are currently available and to help construct valid API requests.

> **Tip:** You can load and parse this JSON in your application to dynamically present options or validate inputs for your users.




---

## Support

If you encounter any issues or have questions, please contact our support team at [support@spacely.ai](mailto:support@spacely.ai).


