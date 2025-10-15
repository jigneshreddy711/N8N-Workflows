# AI-Powered Workflow Automation

This repository contains two n8n workflows designed to leverage AI for practical automation tasks: **Meal AI** for nutritional analysis of meal images and **LinkedIn Post Automation** for generating and publishing professional LinkedIn posts with AI-generated content and images.

## Workflows Overview

### 1. Meal AI Workflow
The Meal AI workflow processes images of meals to identify food items, estimate portion sizes, and calculate nutritional content using AI and standard nutritional databases.

- **Functionality**:
  - Triggered via a webhook to accept meal image inputs.
  - Uses Google Gemini (models/gemini-2.5-flash-preview-05-20) to identify visible food items and approximate portion sizes (e.g., "200g grilled chicken breast, 1 cup rice").
  - Estimates macronutrients (calories, protein, carbohydrates, fats) and micronutrients (e.g., sodium, vitamins) based on USDA nutritional data.
  - Aggregates nutritional totals for the entire meal.
  - Handles poor image quality by noting assumptions or uncertainties.
  - Outputs structured JSON with nutritional data for downstream use.

- **Use Case**: Ideal for health apps, dietary tracking, or nutrition consulting services requiring automated meal analysis.

### 2. LinkedIn Post Automation Workflow
The LinkedIn Post Automation workflow generates professional LinkedIn posts, including text and images, based on user-submitted topics, and publishes them directly to LinkedIn.

- **Functionality**:
  - Triggered by a form submission capturing a user-defined topic.
  - Maps the topic to a variable (`chatInput`) and processes it using Google Gemini (models/gemini-2.0-flash) to generate:
    - Engaging post content with a professional tone, actionable insights, and a call-to-action (CTA) with relevant hashtags.
    - A visual prompt for a professional image aligned with LinkedIn’s corporate aesthetic (blues, whites, grays; minimalist design).
  - Sends the image prompt to the gen-imager API to create a high-quality image.
  - Normalizes AI output and decodes base64 image data for LinkedIn compatibility.
  - Publishes the post and image directly to LinkedIn via the LinkedIn API.

- **Use Case**: Streamlines content creation for digital marketing, personal branding, or social media management.

## Prerequisites
- **n8n**: Install and configure n8n (cloud or self-hosted) to run the workflows.
- **API Credentials**:
  - Google Gemini API key for AI processing (both workflows).
  - Gen-imager API key (RapidAPI) for image generation (LinkedIn workflow).
  - LinkedIn OAuth2 API credentials for post publishing (LinkedIn workflow).
- **Dependencies**: Ensure access to n8n nodes (`@n8n/n8n-nodes-langchain`, `n8n-nodes-base`) and a stable internet connection for API calls.

## Installation
1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   ```
2. **Import Workflows**:
   - Open n8n and import the `meal-ai.json` and `gemini-linkedin.json` files from the repository.
3. **Configure Credentials**:
   - Set up Google Gemini API credentials in n8n for both workflows.
   - Add gen-imager API key for the LinkedIn workflow’s `Text to image` node.
   - Configure LinkedIn OAuth2 credentials for the `Create a post` node.
4. **Activate Workflows**:
   - Enable the `Meal AI` workflow (active by default) or the `LinkedIn Post Automation` workflow (disabled by default; enable in n8n settings).

## Usage

### Meal AI Workflow
1. Send a POST request to the webhook endpoint (`meal-ai`) with a meal image.
2. The workflow processes the image, identifies food items, and returns a JSON object with nutritional data, e.g.:
   ```json
   {
     "success": true,
     "data": {
       "calories": 450,
       "protein": 20,
       "carbs": 50,
       "fat": 15,
       "vitamins": {
         "vitaminA": 10,
         "vitaminC": 5
       }
     }
   }
   ```

### LinkedIn Post Automation Workflow
1. Submit a topic via the form (e.g., "AI in Business").
2. The workflow generates a professional LinkedIn post and image prompt, sends the prompt to the gen-imager API, and publishes the post to LinkedIn.
3. Output includes the post text and image, e.g.:
   ```json
   {
     "post_content": "The Future of AI in Business: How It is Transforming Industries \n...",
     "image_prompt": "A modern co-working space with AI assistants and human employees..."
   }
   ```

## Project Structure
- `meal-ai.json`: Workflow configuration for nutritional analysis.
- `gemini-linkedin.json`: Workflow configuration for LinkedIn post automation.
- `README.md`: This file, providing documentation for setup and usage.

## Notes
- Ensure image inputs for the Meal AI workflow are clear for accurate food identification.
- Customize the LinkedIn post tone or image style by modifying the `AI Agent` node’s system message.
- Monitor API usage to stay within rate limits for Google Gemini and gen-imager APIs.
- For LinkedIn publishing, verify OAuth2 token validity and permissions.

## Contributing
Contributions are welcome! Please submit a pull request or open an issue for bug reports, feature requests, or improvements.

## License
This project is licensed under the MIT License.
