  * [Safety settings](https://www.google.com/search?q=%23safety-settings)
      * [Safety filters](https://www.google.com/search?q=%23safety-filters)
      * [Content safety filtering level](https://www.google.com/search?q=%23content-safety-filtering-level)
      * [Safety filtering per request](https://www.google.com/search?q=%23safety-filtering-per-request)
      * [Safety feedback](https://www.google.com/search?q=%23safety-feedback)
      * [Adjust safety settings](https://www.google.com/search?q=%23adjust-safety-settings)
          * [Google AI Studio](https://www.google.com/search?q=%23google-ai-studio)
          * [Gemini API SDKs](https://www.google.com/search?q=%23gemini-api-sdks)
      * [Next steps](https://www.google.com/search?q=%23next-steps)

# Safety settings

The Gemini API provides safety settings that you can adjust during the prototyping stage to determine if your application requires more or less restrictive safety configuration. You can adjust these settings across five filter categories to restrict or allow certain types of content.

This guide covers how the Gemini API handles safety settings and filtering and how you can change the safety settings for your application.

Note: Applications that use less restrictive safety settings may be subject to review. See the Terms of Service for more information.

## Safety filters

The Gemini API's adjustable safety filters cover the following categories:

| Category           | Description                                                   |
| :----------------- | :------------------------------------------------------------ |
| Harassment         | Negative or harmful comments targeting identity and/or protected attributes. |
| Hate speech        | Content that is rude, disrespectful, or profane.              |
| Sexually explicit  | Contains references to sexual acts or other lewd content.     |
| Dangerous          | Promotes, facilitates, or encourages harmful acts.           |
| Civic integrity    | Election-related queries.                                     |

These categories are defined in `HarmCategory`. The Gemini models only support `HARM_CATEGORY_HARASSMENT`, `HARM_CATEGORY_HATE_SPEECH`, `HARM_CATEGORY_SEXUALLY_EXPLICIT`, `HARM_CATEGORY_DANGEROUS_CONTENT`, and `HARM_CATEGORY_CIVIC_INTEGRITY`. All other categories are used only by PaLM 2 (Legacy) models.

You can use these filters to adjust what's appropriate for your use case. For example, if you're building video game dialogue, you may deem it acceptable to allow more content that's rated as Dangerous due to the nature of the game.

In addition to the adjustable safety filters, the Gemini API has built-in protections against core harms, such as content that endangers child safety. These types of harm are always blocked and cannot be adjusted.

## Content safety filtering level

The Gemini API categorizes the probability level of content being unsafe as `HIGH`, `MEDIUM`, `LOW`, or `NEGLIGIBLE`.

The Gemini API blocks content based on the probability of content being unsafe and not the severity. This is important to consider because some content can have low probability of being unsafe even though the severity of harm could still be high. For example, comparing the sentences:

  * The robot punched me.
  * The robot slashed me up.

The first sentence might result in a higher probability of being unsafe, but you might consider the second sentence to be a higher severity in terms of violence. Given this, it is important that you carefully test and consider what the appropriate level of blocking is needed to support your key use cases while minimizing harm to end users.

## Safety filtering per request

You can adjust the safety settings for each request you make to the API. When you make a request, the content is analyzed and assigned a safety rating. The safety rating includes the category and the probability of the harm classification. For example, if the content was blocked due to the harassment category having a high probability, the safety rating returned would have category equal to `HARASSMENT` and harm probability set to `HIGH`.

By default, safety settings block content (including prompts) with medium or higher probability of being unsafe across any filter. This baseline safety is designed to work for most use cases, so you should only adjust your safety settings if it's consistently required for your application.

The following table describes the block settings you can adjust for each category. For example, if you set the block setting to `Block few` for the `Hate speech` category, everything that has a high probability of being hate speech content is blocked. But anything with a lower probability is allowed.

| Threshold (Google AI Studio) | Threshold (API)           | Description                                       |
| :--------------------------- | :------------------------ | :------------------------------------------------ |
| Block none                   | `BLOCK_NONE`              | Always show regardless of probability of unsafe content |
| Block few                    | `BLOCK_ONLY_HIGH`         | Block when high probability of unsafe content     |
| Block some                   | `BLOCK_MEDIUM_AND_ABOVE`  | Block when medium or high probability of unsafe content |
| Block most                   | `BLOCK_LOW_AND_ABOVE`     | Block when low, medium or high probability of unsafe content |
| N/A                          | `HARM_BLOCK_THRESHOLD_UNSPECIFIED` | Threshold is unspecified, block using default threshold |

If the threshold is not set, the default block threshold is `Block none` (for `gemini-1.5-pro-002` and `gemini-1.5-flash-002` and all newer stable GA models) or `Block some` (in all other models) for all categories except the `Civic integrity` category.

The default block threshold for the `Civic integrity` category is `Block none` (for `gemini-2.0-flash-001` aliased as `gemini-2.0-flash`, `gemini-2.0-pro-exp-02-05`, and `gemini-2.0-flash-lite`) both for Google AI Studio and the Gemini API, and `Block most` for all other models in Google AI Studio only.

You can set these settings for each request you make to the generative service. See the `HarmBlockThreshold` API reference for details.

## Safety feedback

`generateContent` returns a `GenerateContentResponse` which includes safety feedback.

Prompt feedback is included in `promptFeedback`. If `promptFeedback.blockReason` is set, then the content of the prompt was blocked.

Response candidate feedback is included in `Candidate.finishReason` and `Candidate.safetyRatings`. If response content was blocked and the `finishReason` was `SAFETY`, you can inspect `safetyRatings` for more details. The content that was blocked is not returned.

## Adjust safety settings

This section covers how to adjust the safety settings in both Google AI Studio and in your code.

### Google AI Studio

You can adjust safety settings in Google AI Studio, but you cannot turn them off.

Click **Edit safety settings** in the **Run settings** panel to open the **Run safety settings** modal. In the modal, you can use the sliders to adjust the content filtering level per safety category:

[Image of Google AI Studio safety settings sliders]

Note: If you set any of the category filters to **Block none**, Google AI Studio will display a reminder about the Gemini API's Terms of Service with respect to safety settings.

When you send a request (for example, by asking the model a question), a `warning` **No Content** message appears if the request's content is blocked. To see more details, hold the pointer over the **No Content** text and click `warning` **Safety**.

### Gemini API SDKs

The following code snippet shows how to set safety settings in your `GenerateContent` call. This sets the thresholds for the harassment (`HARM_CATEGORY_HARASSMENT`) and hate speech (`HARM_CATEGORY_HATE_SPEECH`) categories. For example, setting these categories to `BLOCK_LOW_AND_ABOVE` blocks any content that has a low or higher probability of being harassment or hate speech. To understand the threshold settings, see Safety filtering per request.

Python Go JavaScript Dart (Flutter) Kotlin Java REST

```bash
echo '{    "safetySettings": [        {"category": "HARM_CATEGORY_HARASSMENT", "threshold": "BLOCK_ONLY_HIGH"},        {"category": "HARM_CATEGORY_HATE_SPEECH", "threshold": "BLOCK_MEDIUM_AND_ABOVE"}    ],    "contents": [{        "parts":[{            "text": "'I support Martians Soccer Club and I think Jupiterians Football Club sucks! Write a ironic phrase about them.'"}]}]}' > request.json

curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=$GEMINI_API_KEY" \
    -H 'Content-Type: application/json' \
    -X POST \
    -d @request.json 2> /dev/null
```

## Next steps

  * See the API reference to learn more about the full API.
  * Review the safety guidance for a general look at safety considerations when developing with LLMs.
  * Learn more about assessing probability versus severity from the Jigsaw team
  * Learn more about the products that contribute to safety solutions like the Perspective API.
  * You can use these safety settings to create a toxicity classifier. See the classification example to get started.
