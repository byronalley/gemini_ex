# Rate limits

Rate limits regulate the number of requests you can make to the Gemini API within a given timeframe. These limits help ensure fair usage, protect against abuse, and help maintain system performance for all users.

## How rate limits work

Rate limits are measured across four dimensions:

* Requests per minute (RPM)
* Requests per day (RPD)
* Tokens per minute (TPM)
* Tokens per day (TPD)

Your usage is evaluated against each limit, and exceeding any of them will trigger a rate limit error. For example, if your RPM limit is 20, making 21 requests within a minute will result in an error, even if you haven't exceeded your TPM or other limits.

Rate limits are applied per project, not per API key.

Limits vary depending on the specific model being used, and some limits only apply to specific models. For example, Images per minute, or IPM, is only calculated for models capable of generating images (Imagen 3), but is conceptually similar to TPM.

Rate limits are more restricted for experimental and preview models.

## Usage tiers

Rate limits are tied to the project's usage tier. As your API usage and spending increase, you'll have an option to upgrade to a higher tier with increased rate limits.

| Tier         | Qualifications                                          |
| :----------- | :------------------------------------------------------ |
| Free         | Users in eligible countries                             |
| Tier 1       | Billing account linked to the project                   |
| Tier 2       | Total spend: $250 + at least 30 days since successful payment |
| Tier 3       | Total spend: $1,000 + at least 30 days since successful payment |

When you request an upgrade, our automated abuse protection system performs additional checks. While meeting the stated qualification criteria is generally sufficient for approval, in rare cases an upgrade request may be denied based on other factors identified during the review process.

This system helps ensure the security and integrity of the Gemini API platform for all users.

## Current rate limits

| Model                            | Free Tier   | Tier 1      | Tier 2      | Tier 3      |
| :------------------------------- | :---------- | :---------- | :---------- | :---------- |
|                                  | RPM / TPM / RPD | RPM / TPM / RPD | RPM / TPM / RPD | RPM / TPM / RPD |
| Gemini 2.5 Flash Preview 04-17   | 10 / 250,000 / 500 | ---         | ---         | ---         |
| Gemini 2.5 Pro Preview 05-06     | ---         | ---         | ---         | ---         |
| Gemini 2.0 Flash                 | 15 / 1,000,000 / 1,500 | ---         | ---         | ---         |
| Gemini 2.0 Flash Preview Image Generation | 10 / 200,000 / 100 | ---         | ---         | ---         |
| Gemini 2.0 Flash Experimental    | 10 / 1,000,000 / 1,000 | ---         | ---         | ---         |
| Gemini 2.0 Flash-Lite            | 30 / 1,000,000 / 1,500 | ---         | ---         | ---         |
| Gemini 1.5 Flash                 | 15 / 250,000 / 500 | ---         | ---         | ---         |
| Gemini 1.5 Flash-8B              | 15 / 250,000 / 500 | ---         | ---         | ---         |
| Gemini 1.5 Pro                   | ---         | ---         | ---         | ---         |
| Veo 2                            | ---         | ---         | ---         | ---         |
| Imagen 3                         | ---         | ---         | ---         | ---         |
| Gemma 3                          | 30 / 15,000 / 14,400 | ---         | ---         | ---         |
| Gemini Embedding Experimental 03-07 | 5 / -- / 100 | ---         | ---         | ---         |

Specified rate limits are not guaranteed and actual capacity may vary.

## Live API rate limits

| Tier               | Free Tier | Tier 1      | Tier 2      | Tier 3      |
| :----------------- | :-------- | :---------- | :---------- | :---------- |
|                    | Concurrent Sessions / TPM | Concurrent Sessions / TPM | Concurrent Sessions / TPM | Concurrent Sessions / TPM |
| Number of concurrent sessions / TPM | 3 / 1,000,000 | ---         | ---         | ---         |

Specified rate limits are not guaranteed and actual capacity may vary.

## How to upgrade to the next tier

The Gemini API uses Cloud Billing for all billing services. To transition from the Free tier to a paid tier, you must first enable Cloud Billing for your Google Cloud project.

Once your project meets the specified criteria, it becomes eligible for an upgrade to the next tier. To request an upgrade, follow these steps:

1.  Navigate to the API keys page in AI Studio.
2.  Locate the project you want to upgrade and click "Upgrade". The "Upgrade" option will only show up for projects that meet next tier qualifications.
3.  After a quick validation, the project will be upgraded to the next tier.

## Request a rate limit increase

Each model variation has an associated rate limit (requests per minute, RPM). For details on those rate limits, see Gemini models.

### Request paid tier rate limit increase

We offer no guarantees about increasing your rate limit, but we'll do our best to review your request and reach out to you if we're able to accommodate your capacity needs.
