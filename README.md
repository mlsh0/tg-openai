# ChatGPT Telegram Bot
A [Telegram bot](https://core.telegram.org/bots/api) that integrates with OpenAI's _official_ [ChatGPT](https://openai.com/blog/chatgpt/), [DALL·E](https://openai.com/product/dall-e-2) and [Whisper](https://openai.com/research/whisper) APIs to provide answers. Ready to use with minimal configuration required.
## Prerequisites
- [Docker](https://get.docker.com/)
- [Telegram bot](https://core.telegram.org/bots#6-botfather) and its token (see [tutorial](https://core.telegram.org/bots/tutorial#obtain-your-bot-token))
- OpenAI API

### Configuration
Customize the configuration by copying `.env.example` and renaming it to `.env`, then editing the required parameters as desired:

| Parameter                   | Description                                                                                                                                                                                                                   |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `OPENAI_API_KEY`            | Your OpenAI API key, you can get it from [here](https://platform.openai.com/account/api-keys)                                                                                                                                 |
| `TELEGRAM_BOT_TOKEN`        | Your Telegram bot's token, obtained using [BotFather](http://t.me/botfather) (see [tutorial](https://core.telegram.org/bots/tutorial#obtain-your-bot-token))                                                                  |
| `ADMIN_USER_IDS`            | Telegram user IDs of admins. These users have access to special admin commands, information and no budget restrictions. Admin IDs don't have to be added to `ALLOWED_TELEGRAM_USER_IDS`. **Note**: by default, no admin (`-`) |
| `ALLOWED_TELEGRAM_USER_IDS` | A comma-separated list of Telegram user IDs that are allowed to interact with the bot (use [getidsbot](https://t.me/getidsbot) to find your user ID). **Note**: by default, *everyone* is allowed (`*`)                       |

### Optional configuration
The following parameters are optional and can be set in the `.env` file:

#### Budgets
| Parameter             | Description                                                                                                                                                                                                                                                                                              | Default value      |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| `BUDGET_PERIOD`       | Determines the time frame all budgets are applied to. Available periods: `daily` *(resets budget every day)*, `monthly` *(resets budgets on the first of each month)*, `all-time` *(never resets budget)*. See the  for more information                                                                 | `monthly`          |
| `USER_BUDGETS`        | A comma-separated list of $-amounts per user from list `ALLOWED_TELEGRAM_USER_IDS` to set custom usage limit of OpenAI API costs for each. For `*`- user lists the first `USER_BUDGETS` value is given to every user. **Note**: by default, *no limits* for any user (`*`). See the for more information | `*`                |
| `GUEST_BUDGET`        | $-amount as usage limit for all guest users. Guest users are users in group chats that are not in the `ALLOWED_TELEGRAM_USER_IDS` list. Value is ignored if no usage limits are set in user budgets (`USER_BUDGETS`=`*`).                                                                                | `100.0`            |
| `TOKEN_PRICE`         | $-price per 1000 tokens used to compute cost information in usage statistics. Source: https://openai.com/pricing                                                                                                                                                                                         | `0.002`            |
| `IMAGE_PRICES`        | A comma-separated list with 3 elements of prices for the different image sizes: `256x256`, `512x512` and `1024x1024`. Source: https://openai.com/pricing                                                                                                                                                 | `0.016,0.018,0.02` |
| `TRANSCRIPTION_PRICE` | USD-price for one minute of audio transcription. Source: https://openai.com/pricing                                                                                                                                                                                                                      | `0.006`            |
| `VISION_TOKEN_PRICE`  | USD-price per 1K tokens of image interpretation. Source: https://openai.com/pricing                                                                                                                                                                                                                      | `0.01`             |
| `TTS_PRICES`          | A comma-separated list with prices for the tts models: `tts-1`, `tts-1-hd`. Source: https://openai.com/pricing                                                                                                                                                                                           | `0.015,0.030`      |

#### Additional optional configuration options
| Parameter                           | Description                                                                                                                                                                                                                          | Default value                      |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------- |
| `ENABLE_QUOTING`                    | Whether to enable message quoting in private chats                                                                                                                                                                                   | `true`                             |
| `ENABLE_IMAGE_GENERATION`           | Whether to enable image generation via the `/image` command                                                                                                                                                                          | `true`                             |
| `ENABLE_TRANSCRIPTION`              | Whether to enable transcriptions of audio and video messages                                                                                                                                                                         | `true`                             |
| `ENABLE_TTS_GENERATION`             | Whether to enable text to speech generation via the `/tts`                                                                                                                                                                           | `true`                             |
| `ENABLE_VISION`                     | Whether to enable vision capabilities in supported models                                                                                                                                                                            | `true`                             |
| `PROXY`                             | Proxy to be used for OpenAI and Telegram bot (e.g. `http://localhost:8080`)                                                                                                                                                          | -                                  |
| `OPENAI_PROXY`                      | Proxy to be used only for OpenAI (e.g. `http://localhost:8080`)                                                                                                                                                                      | -                                  |
| `TELEGRAM_PROXY`                    | Proxy to be used only for Telegram bot (e.g. `http://localhost:8080`)                                                                                                                                                                | -                                  |
| `OPENAI_MODEL`                      | The OpenAI model to use for generating responses. You can find all available models [here](https://platform.openai.com/docs/models/)                                                                                                 | `gpt-3.5-turbo`                    |
| `OPENAI_BASE_URL`                   | Endpoint URL for unofficial OpenAI-compatible APIs (e.g., LocalAI or text-generation-webui)                                                                                                                                          | Default OpenAI API URL             |
| `ASSISTANT_PROMPT`                  | A system message that sets the tone and controls the behavior of the assistant                                                                                                                                                       | `You are a helpful assistant.`     |
| `SHOW_USAGE`                        | Whether to show OpenAI token usage information after each response                                                                                                                                                                   | `false`                            |
| `STREAM`                            | Whether to stream responses. **Note**: incompatible, if enabled, with `N_CHOICES` higher than 1                                                                                                                                      | `true`                             |
| `MAX_TOKENS`                        | Upper bound on how many tokens the ChatGPT API will return                                                                                                                                                                           | `1200` for GPT-3, `2400` for GPT-4 |
| `VISION_MAX_TOKENS`                 | Upper bound on how many tokens vision models will return                                                                                                                                                                             | `300` for gpt-4-vision-preview     |
| `VISION_MODEL`                      | The Vision to Speech model to use. Allowed values: `gpt-4-vision-preview`                                                                                                                                                            | `gpt-4-vision-preview`             |
| `ENABLE_VISION_FOLLOW_UP_QUESTIONS` | If true, once you send an image to the bot, it uses the configured VISION_MODEL until the conversation ends. Otherwise, it uses the OPENAI_MODEL to follow the conversation. Allowed values: `true` or `false`                       | `true`                             |
| `MAX_HISTORY_SIZE`                  | Max number of messages to keep in memory, after which the conversation will be summarised to avoid excessive token usage                                                                                                             | `15`                               |
| `MAX_CONVERSATION_AGE_MINUTES`      | Maximum number of minutes a conversation should live since the last message, after which the conversation will be reset                                                                                                              | `180`                              |
| `VOICE_REPLY_WITH_TRANSCRIPT_ONLY`  | Whether to answer to voice messages with the transcript only or with a ChatGPT response of the transcript                                                                                                                            | `false`                            |
| `VOICE_REPLY_PROMPTS`               | A semicolon separated list of phrases (i.e. `Hi bot;Hello chat`). If the transcript starts with any of them, it will be treated as a prompt even if `VOICE_REPLY_WITH_TRANSCRIPT_ONLY` is set to `true`                              | -                                  |
| `VISION_PROMPT`                     | A phrase (i.e. `What is in this image`). The vision models use it as prompt to interpret a given image. If there is caption in the image sent to the bot, that supersedes this parameter                                             | `What is in this image`            |
| `N_CHOICES`                         | Number of answers to generate for each input message. **Note**: setting this to a number higher than 1 will not work properly if `STREAM` is enabled                                                                                 | `1`                                |
| `TEMPERATURE`                       | Number between 0 and 2. Higher values will make the output more random                                                                                                                                                               | `1.0`                              |
| `PRESENCE_PENALTY`                  | Number between -2.0 and 2.0. Positive values penalize new tokens based on whether they appear in the text so far                                                                                                                     | `0.0`                              |
| `FREQUENCY_PENALTY`                 | Number between -2.0 and 2.0. Positive values penalize new tokens based on their existing frequency in the text so far                                                                                                                | `0.0`                              |
| `IMAGE_FORMAT`                      | The Telegram image receive mode. Allowed values: `document` or `photo`                                                                                                                                                               | `photo`                            |
| `IMAGE_MODEL`                       | The DALL·E model to be used. Available models: `dall-e-2` and `dall-e-3`, find current available models [here](https://platform.openai.com/docs/models/dall-e)                                                                       | `dall-e-2`                         |
| `IMAGE_QUALITY`                     | Quality of DALL·E images, only available for `dall-e-3`-model. Possible options: `standard` or `hd`, beware of [pricing differences](https://openai.com/pricing#image-models).                                                       | `standard`                         |
| `IMAGE_STYLE`                       | Style for DALL·E image generation, only available for `dall-e-3`-model. Possible options: `vivid` or `natural`. Check availbe styles [here](https://platform.openai.com/docs/api-reference/images/create).                           | `vivid`                            |
| `IMAGE_SIZE`                        | The DALL·E generated image size. Must be `256x256`, `512x512`, or `1024x1024` for dall-e-2. Must be `1024x1024` for dall-e-3 models.                                                                                                 | `512x512`                          |
| `VISION_DETAIL`                     | The detail parameter for vision models, explained [Vision Guide](https://platform.openai.com/docs/guides/vision). Allowed values: `low` or `high`                                                                                    | `auto`                             |
| `GROUP_TRIGGER_KEYWORD`             | If set, the bot in group chats will only respond to messages that start with this keyword                                                                                                                                            | -                                  |
| `IGNORE_GROUP_TRANSCRIPTIONS`       | If set to true, the bot will not process transcriptions in group chats                                                                                                                                                               | `true`                             |
| `IGNORE_GROUP_VISION`               | If set to true, the bot will not process vision queries in group chats                                                                                                                                                               | `true`                             |
| `BOT_LANGUAGE`                      | Language of general bot messages. Currently available: `en`, `de`, `ru`, `tr`, `it`, `fi`, `es`, `id`, `nl`, `zh-cn`, `zh-tw`, `vi`, `fa`, `pt-br`, `uk`, `ms`, `uz`, `ar`.                                                          | `en`                               |
| `WHISPER_PROMPT`                    | To improve the accuracy of Whisper's transcription service, especially for specific names or terms, you can set up a custom message.  [Speech to text - Prompting](https://platform.openai.com/docs/guides/speech-to-text/prompting) | `-`                                |
| `TTS_VOICE`                         | The Text to Speech voice to use. Allowed values: `alloy`, `echo`, `fable`, `onyx`, `nova`, or `shimmer`                                                                                                                              | `alloy`                            |
| `TTS_MODEL`                         | The Text to Speech model to use. Allowed values: `tts-1` or `tts-1-hd`                                                                                                                                                               | `tts-1`                            |

Check out the [official API reference](https://platform.openai.com/docs/api-reference/chat) for more details.

#### Functions
| Parameter                         | Description                                                                                                                                      | Default value                       |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------- |
| `ENABLE_FUNCTIONS`                | Whether to use functions (aka plugins). You can read more about functions [here](https://openai.com/blog/function-calling-and-other-api-updates) | `true` (if available for the model) |
| `FUNCTIONS_MAX_CONSECUTIVE_CALLS` | Maximum number of back-to-back function calls to be made by the model in a single response, before displaying a user-facing message              | `10`                                |
| `PLUGINS`                         | List of plugins to enable (see below for a full list), e.g: `PLUGINS=wolfram,weather`                                                            | -                                   |
| `SHOW_PLUGINS_USED`               | Whether to show which plugins were used for a response                                                                                           | `false`                             |

#### Available plugins
| Name                  | Description                                                                                                                        | Required environment variable(s) | Dependency     |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- | -------------- |
| `wolfram`             | WolframAlpha queries (powered by [WolframAlpha](https://www.wolframalpha.com))                                                     | `WOLFRAM_APP_ID`                 | `wolframalpha` |
| `worldtimeapi`        | Get latest world time (powered by [WorldTimeAPI](https://worldtimeapi.org/)) - by [@noriellecruz](https://github.com/noriellecruz) | `WORLDTIME_DEFAULT_TIMEZONE`     |                |
| `deepl_translate`     | Translate text to any language (powered by [DeepL](https://deepl.com)) - by [@LedyBacer](https://github.com/LedyBacer)             | `DEEPL_API_KEY`                  |                |
| `gtts_text_to_speech` | Text to speech (powered by Google Translate APIs)                                                                                  | -                                | `gtts`         |
| `auto_tts`            | Text to speech using OpenAI APIs - by [@Jipok](https://github.com/Jipok)                                                           | -                                |                |

#### Environment variables  
`WORLDTIME_DEFAULT_TIMEZONE`      | Default timezone to use (required only for the `worldtimeapi` plugin                       
                                                                  
`DEEPL_API_KEY`                   | DeepL API key (required for the `deepl` plugin, you can get one [here](https://www.deepl.com/pro-api?cta=header-pro-api)                                                        

### Installing
Clone the repository and navigate to the project directory:

#### Using Docker Compose

Run the following command to build and run the Docker image:
```shell
docker compose up -d
```

## Credits
- [ChatGPT](https://chat.openai.com/chat) from [OpenAI](https://openai.com)
- [python-telegram-bot](https://python-telegram-bot.org)
- [jiaaro/pydub](https://github.com/jiaaro/pydub)
- [chatgpt-telegram-bot](https://github.com/n3d1117/chatgpt-telegram-bot)
- [@CameronJGrant (fixed curl_cffi)](https://github.com/CameronJGrant)

## Disclaimer
This is a personal project and is not affiliated with OpenAI in any way.

## License
This project is released under the terms of the GPL 2.0 license. For more information, see the [LICENSE](LICENSE) file included in the repository.
