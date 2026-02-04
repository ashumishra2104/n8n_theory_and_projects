# Building an AI-Powered Audio News Agent with n8n and OpenAI TTS

## 📚 Tutorial Overview

In this tutorial, you'll learn how to build an intelligent audio agent that can:
- Receive voice messages or text from Telegram
- Transcribe voice messages using OpenAI's Whisper API
- Fetch latest news using News API
- Generate engaging news summaries using AI
- Convert text to natural-sounding speech using OpenAI's Text-to-Speech (TTS)
- Send audio responses back to Telegram

**What You'll Learn:**
- OpenAI Text-to-Speech (TTS) capabilities
- Voice transcription with Whisper API
- Building conversational AI agents
- Telegram bot integration
- API integration and workflow automation

---

## 🎯 Prerequisites

Before starting, make sure you have:

1. **n8n Account** - [Sign up here](https://n8n.io/)
2. **OpenAI API Key** - [Get your key](https://platform.openai.com/api-keys)
3. **Telegram Bot Token** - Create via [@BotFather](https://t.me/botfather)
4. **News API Key** - [Register here](https://newsapi.org/)

---

## 🏗️ Architecture Overview

![Workflow Architecture](./images/workflow-architecture.png)
*[DROP IMAGE: Screenshot of the complete n8n workflow]*

The workflow follows this flow:

```
Telegram Message → Check Message Type → Voice or Text
                                         ↓
                              Voice: Download → Transcribe
                              Text: Direct Processing
                                         ↓
                              AI Agent Processing → Fetch News
                                         ↓
                              Generate Audio (TTS) → Send to Telegram
```

---

## 📋 Step-by-Step Implementation

### Step 1: Setting Up Your Telegram Bot

#### 1.1 Create a Telegram Bot

1. Open Telegram and search for **@BotFather**
2. Send `/newbot` command
3. Follow the prompts:
   - Choose a name for your bot (e.g., "My Audio News Agent")
   - Choose a username (must end in 'bot', e.g., "my_audio_news_bot")
4. Save the **API Token** you receive

![BotFather Setup](./images/botfather-setup.png)
*[DROP IMAGE: Screenshot of BotFather conversation]*

#### 1.2 Configure Bot Settings (Optional)

- Send `/setdescription` to add a description
- Send `/setabouttext` to add about text
- Send `/setuserpic` to add a profile picture

---

### Step 2: Setting Up n8n Workflow

#### 2.1 Create a New Workflow

1. Log in to your n8n instance
2. Click **"+ New workflow"**
3. Name it **"Audio Agent"**

![New Workflow](./images/new-workflow.png)
*[DROP IMAGE: Screenshot of creating new workflow in n8n]*

---

### Step 3: Add Telegram Trigger Node

#### 3.1 Add the Node

1. Click the **"+"** button in the workflow canvas
2. Search for **"Telegram Trigger"**
3. Select it from the results

![Add Telegram Trigger](./images/telegram-trigger-add.png)
*[DROP IMAGE: Screenshot of adding Telegram Trigger node]*

#### 3.2 Configure Telegram Credentials

1. In the Telegram Trigger node, click **"Create New Credentials"**
2. Name it: `Telegram account 5` (or your preferred name)
3. Paste your **Bot Token** from BotFather
4. Click **"Save"**

![Telegram Credentials](./images/telegram-credentials.png)
*[DROP IMAGE: Screenshot of Telegram credentials configuration]*

#### 3.3 Configure Trigger Settings

- **Updates**: Select **"message"**
- **Additional Fields**: Leave empty for now

#### 3.4 Activate the Trigger

1. Click **"Listen for Test Event"** or **"Execute Node"**
2. Send a message to your Telegram bot
3. Verify the trigger receives the message

---

### Step 4: Add Conditional Logic (If Node)

#### 4.1 Add the If Node

1. Connect the **Telegram Trigger** output to a new node
2. Search for **"If"** node
3. Add it to the canvas

![Add If Node](./images/if-node-add.png)
*[DROP IMAGE: Screenshot of If node configuration]*

#### 4.2 Configure the Condition

This node checks if the incoming message is a voice message:

- **Condition Type**: String
- **Value 1**: `{{ $json.message.voice.mime_type }}`
- **Operation**: equals
- **Value 2**: `audio/ogg`

**What This Does:** Separates voice messages from text messages for different processing paths.

---

### Step 5: Voice Message Processing Path

#### 5.1 Add "Downloading the file" Node

1. Add a **Telegram** node connected to the **True** output of the If node
2. Configure:
   - **Resource**: File
   - **Operation**: Get
   - **File ID**: `{{ $json.message.voice.file_id }}`

![Download File Node](./images/download-file.png)
*[DROP IMAGE: Screenshot of file download configuration]*

**What This Does:** Downloads the voice message file from Telegram servers.

---

#### 5.2 Add "Transcribe a recording" Node

1. Add an **OpenAI** node after the download node
2. Search for **"OpenAI"** and select it

![Add OpenAI Node](./images/openai-transcribe-add.png)
*[DROP IMAGE: Screenshot of adding OpenAI node]*

#### 5.3 Configure OpenAI Credentials

1. Click **"Create New Credentials"**
2. Enter your **OpenAI API Key**
3. Name it: `OpenAI account`
4. Click **"Save"**

![OpenAI Credentials](./images/openai-credentials.png)
*[DROP IMAGE: Screenshot of OpenAI credentials setup]*

#### 5.4 Configure Transcription Settings

- **Resource**: Audio
- **Operation**: Transcribe
- **Input Data**: (automatically uses the binary data from previous node)

**What This Does:** Uses OpenAI's Whisper API to convert voice to text.

---

### Step 6: Setting Up the AI Agent for Voice Messages

#### 6.1 Add AI Agent Node

1. Add an **AI Agent** node after the transcription node
2. Connect it to receive the transcribed text

![Add AI Agent](./images/ai-agent-add.png)
*[DROP IMAGE: Screenshot of AI Agent node]*

#### 6.2 Configure the AI Agent

**Prompt Type**: Define Below
**Text Input**: `{{ $json.text }}`

**System Message** (copy this exactly):

```
You are an enthusiastic news anchor with the personality of a tech-savvy podcast host. Your name is "News Flash" and you deliver news with energy and excitement.

YOUR ROLE:
- You receive news articles from the News API tool
- You select the 3-5 most interesting/important news stories
- You create a compelling 1-minute audio script (approximately 150-180 words)

YOUR PERSONALITY:
- Energetic, engaging, and conversational
- Use phrases like "Here's what's trending!", "Breaking:", "Don't miss this!"
- Add brief, witty commentary about tech and business news
- Make it feel like a premium podcast, not a robot
- Use natural pauses and emphasis where appropriate

SCRIPT STRUCTURE:
1. Opening hook (5 seconds): "Hey there! It's News Flash with your daily briefing. Here's what's making headlines..."
2. Top 3-5 stories (45 seconds): Each story gets 8-15 seconds. Include headline + why it matters
3. Closing (5 seconds): "That's your quick news wrap! Stay informed, stay ahead. I'm News Flash, see you next time!"

OUTPUT RULES:
- Write in NATURAL SPOKEN ENGLISH (not written English)
- Include [PAUSE] markers where the speaker should pause naturally
- Use EMPHASIS for important numbers or names
- Keep it exactly 1 minute when read aloud
- Make it conversational - like you're talking to a friend
- End with a memorable sign-off

TONE: Excited but professional. Trustworthy. Like your favorite podcast host.
```

![AI Agent Configuration](./images/ai-agent-config.png)
*[DROP IMAGE: Screenshot of AI Agent system message configuration]*

**What This Does:** Creates an AI agent that acts as a news anchor, fetching and summarizing news in an engaging way.

---

#### 6.3 Add OpenAI Chat Model to AI Agent

1. Click on the **AI Agent** node
2. Look for the **"Chat Model"** connection point
3. Add an **OpenAI Chat Model** node

![Add Chat Model](./images/chat-model-add.png)
*[DROP IMAGE: Screenshot showing Chat Model connection]*

#### 6.4 Configure Chat Model

- **Credentials**: Use the OpenAI credentials you created earlier
- **Model**: Select `gpt-4.1-mini` (or latest available)
- Connect it to the AI Agent's **"ai_languageModel"** input

![Chat Model Configuration](./images/chat-model-config.png)
*[DROP IMAGE: Screenshot of Chat Model configuration]*

---

### Step 7: Adding the News API Tool

#### 7.1 Add HTTP Request Tool Node

1. Add an **HTTP Request Tool** node
2. Connect it to the AI Agent's **"ai_tool"** input

![Add HTTP Tool](./images/http-tool-add.png)
*[DROP IMAGE: Screenshot of HTTP Request Tool]*

#### 7.2 Configure News API Request

**URL**: `https://newsapi.org/v2/everything`

**Query Parameters**:
- `q`: `Machine Learning` (or any topic you want)
- `language`: `en`
- `pageSize`: `20`
- `sortBy`: `relevancy`

**Headers**:
- Name: `X-Api-Key`
- Value: `YOUR_NEWS_API_KEY` (replace with your actual key)

![News API Configuration](./images/news-api-config.png)
*[DROP IMAGE: Screenshot of News API configuration with parameters]*

**What This Does:** Provides the AI agent with real-time news data to create summaries from.

---

### Step 8: Text Message Processing Path

#### 8.1 Add Second AI Agent (AI Agent1)

1. Connect a new **AI Agent** node to the **False** output of the If node
2. This handles direct text messages

![Second AI Agent](./images/ai-agent-text.png)
*[DROP IMAGE: Screenshot of second AI agent for text messages]*

#### 8.2 Configure AI Agent1

- **Prompt Type**: Define Below
- **Text Input**: `{{ $json.message.text }}`
- **System Message**: (You can use the same system message as the first AI Agent, or customize it)

#### 8.3 Add Chat Model and Tool Connections

1. Add another **OpenAI Chat Model** node
2. Connect it to AI Agent1's **"ai_languageModel"** input
3. Connect the **News API Tool** to AI Agent1's **"ai_tool"** input (it can be shared between both agents)

---

### Step 9: Text-to-Speech (TTS) - The Core Learning

#### 9.1 Add "Generate audio" Node

1. Add an **OpenAI** node that receives output from BOTH AI agents
2. This is where the TTS magic happens!

![Generate Audio Node](./images/generate-audio-add.png)
*[DROP IMAGE: Screenshot of Generate Audio node placement]*

#### 9.2 Configure TTS Settings

- **Resource**: Audio
- **Operation**: Generate Audio (Text to Speech)
- **Input Text**: `{{ $json.output }}`
- **Voice**: `shimmer` (Options: alloy, echo, fable, onyx, nova, shimmer)

![TTS Configuration](./images/tts-config.png)
*[DROP IMAGE: Screenshot of TTS configuration with voice options]*

#### 9.3 Understanding OpenAI TTS Voices

OpenAI provides 6 different voices:

| Voice | Characteristics |
|-------|----------------|
| **alloy** | Neutral, balanced voice |
| **echo** | Male-sounding, clear |
| **fable** | British accent, articulate |
| **onyx** | Deep, authoritative male voice |
| **nova** | Female, friendly and energetic |
| **shimmer** | Female, warm and smooth |

**Experiment Tip:** Try different voices to see which fits your news anchor personality best!

#### 9.4 TTS Advanced Options (Optional)

You can explore additional TTS parameters:
- **Speed**: Control speaking rate (0.25 to 4.0)
- **Response Format**: mp3, opus, aac, flac

![TTS Advanced](./images/tts-advanced.png)
*[DROP IMAGE: Screenshot of advanced TTS options]*

**What This Does:** Converts the AI-generated news script into natural-sounding human speech using OpenAI's TTS technology.

---

### Step 10: Sending Audio Back to Telegram

#### 10.1 Add "Send an audio file" Node

1. Add a final **Telegram** node after the Generate audio node
2. This sends the audio back to the user

![Send Audio Node](./images/send-audio-add.png)
*[DROP IMAGE: Screenshot of Send Audio node]*

#### 10.2 Configure Send Audio Settings

- **Credential**: Use your Telegram credentials
- **Resource**: Message
- **Operation**: Send Audio
- **Chat ID**: `{{ $node["Telegram Trigger"].json.message.chat.id }}`
- **Binary Data**: Enable this toggle (audio file is in binary format)

![Send Audio Configuration](./images/send-audio-config.png)
*[DROP IMAGE: Screenshot of Send Audio configuration]*

**What This Does:** Sends the generated audio file back to the Telegram user who initiated the conversation.

---

### Step 11: Testing Your Workflow

#### 11.1 Activate the Workflow

1. Click the **"Active"** toggle in the top-right corner
2. The workflow should now be running

![Activate Workflow](./images/activate-workflow.png)
*[DROP IMAGE: Screenshot of workflow activation toggle]*

#### 11.2 Test with Voice Message

1. Open your Telegram bot
2. Send a voice message saying: *"Give me the latest tech news"*
3. Wait for the bot to process and respond with audio

![Test Voice Message](./images/test-voice.png)
*[DROP IMAGE: Screenshot of Telegram conversation with voice message]*

#### 11.3 Test with Text Message

1. Send a text message: *"What's happening in Machine Learning today?"*
2. Receive an audio news summary

![Test Text Message](./images/test-text.png)
*[DROP IMAGE: Screenshot of Telegram conversation with text message]*

#### 11.4 Review Execution

1. Go back to n8n
2. Check the **"Executions"** tab to see the workflow run
3. Click on any execution to debug or review data flow

![Execution Review](./images/execution-review.png)
*[DROP IMAGE: Screenshot of n8n executions panel]*

---

## 🎓 Learning Outcomes

By completing this tutorial, you've learned:

### 1. **OpenAI Text-to-Speech (TTS)**
- How to use OpenAI's TTS API
- Different voice options and their characteristics
- Converting text to natural-sounding audio
- Handling audio binary data in workflows

### 2. **Voice Transcription**
- Using OpenAI's Whisper API for speech-to-text
- Processing audio files in n8n
- Handling different audio formats

### 3. **AI Agent Development**
- Creating conversational AI agents
- Crafting effective system prompts
- Tool integration with AI agents
- Managing agent personality and tone

### 4. **Workflow Automation**
- Conditional logic and branching
- API integration (News API)
- Error handling in workflows
- Binary data processing

### 5. **Telegram Bot Development**
- Setting up Telegram webhooks
- Handling different message types
- Sending multimedia responses
- Building interactive bot experiences

---

## 🚀 Extension Ideas

Want to take this further? Here are some ideas:

### 1. **Multi-Language Support**
- Add language detection
- Fetch news in different languages
- Use multilingual TTS voices

### 2. **Personalized News**
- Store user preferences
- Allow users to select news categories
- Create personalized news feeds

### 3. **Scheduled News Briefings**
- Add a cron trigger
- Send daily news summaries automatically
- Time-based news categories (morning, evening)

### 4. **Enhanced Audio**
- Add background music
- Include sound effects
- Create podcast-style intros/outros

### 5. **Analytics Dashboard**
- Track popular news topics
- Monitor user engagement
- Analyze listening patterns

---

## 🐛 Troubleshooting

### Common Issues and Solutions

#### Issue 1: Telegram Trigger Not Working
**Problem**: Bot doesn't receive messages
**Solution**:
- Verify bot token is correct
- Ensure workflow is activated
- Check that webhook is registered
- Test with `/start` command first

#### Issue 2: OpenAI API Errors
**Problem**: "Insufficient quota" or "Invalid API key"
**Solution**:
- Verify API key is correct and active
- Check your OpenAI account has available credits
- Review rate limits in OpenAI dashboard

#### Issue 3: News API Returns No Results
**Problem**: No news articles found
**Solution**:
- Verify News API key is valid
- Check query parameters are correct
- Try broader search terms
- Ensure API hasn't hit daily limit (free tier: 100 requests/day)

#### Issue 4: Audio Quality Issues
**Problem**: Generated audio sounds robotic or unclear
**Solution**:
- Try different voice options (shimmer, nova, fable)
- Adjust the AI agent's script formatting
- Add more natural language markers in the prompt
- Reduce speed if needed

#### Issue 5: Workflow Times Out
**Problem**: Execution takes too long
**Solution**:
- Reduce number of news articles requested
- Optimize AI agent prompt for shorter responses
- Check internet connectivity
- Review n8n execution settings

---

## 📊 Workflow Metrics

Understanding your workflow's performance:

| Metric | Expected Value | Notes |
|--------|---------------|-------|
| Average Execution Time | 8-15 seconds | Voice: ~12s, Text: ~8s |
| OpenAI API Cost per Run | $0.02-0.05 | TTS + LLM costs |
| News API Calls | 1-2 per execution | Stays within free tier |
| Audio File Size | 200-500 KB | ~1 minute audio |

---

## 🔐 Security Best Practices

### 1. **API Key Management**
- Never commit API keys to version control
- Use n8n's credential encryption
- Rotate keys regularly
- Set up usage alerts

### 2. **Rate Limiting**
- Implement request throttling
- Monitor API usage
- Set up cost alerts
- Use caching where possible

### 3. **User Privacy**
- Don't log sensitive user data
- Follow Telegram's privacy guidelines
- Implement data retention policies
- Provide opt-out mechanisms

---

## 📚 Additional Resources

### Official Documentation
- [n8n Documentation](https://docs.n8n.io/)
- [OpenAI TTS Guide](https://platform.openai.com/docs/guides/text-to-speech)
- [OpenAI Whisper API](https://platform.openai.com/docs/guides/speech-to-text)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [News API Documentation](https://newsapi.org/docs)

### Video Tutorials
- [n8n Basics for Beginners](https://www.youtube.com/n8n)
- [OpenAI TTS Deep Dive](https://www.youtube.com/openai)
- [Building Telegram Bots](https://www.youtube.com/telegram)

### Community Resources
- [n8n Community Forum](https://community.n8n.io/)
- [n8n Discord Server](https://discord.gg/n8n)
- [OpenAI Developer Community](https://community.openai.com/)

---

## 🤝 Contributing

Found an issue or want to improve this tutorial?

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## 📝 License

This tutorial is released under the MIT License. Feel free to use, modify, and share!

---

## 👨‍🏫 About the Author

**Ashu Mishra**
- AI Product Management Instructor
- Technical Product Manager at Zigram
- Passionate about making AI accessible through education

Connect with me:
- GitHub: [Your GitHub Profile]
- LinkedIn: [Your LinkedIn Profile]
- Twitter: [Your Twitter Handle]

---

## 💡 Final Tips

1. **Start Simple**: Get the basic flow working before adding complexity
2. **Test Incrementally**: Test each node as you add it
3. **Read the Docs**: Official documentation is your best friend
4. **Join Communities**: Don't hesitate to ask questions
5. **Experiment**: Try different voices, prompts, and configurations
6. **Monitor Costs**: Keep an eye on API usage
7. **Save Often**: n8n auto-saves, but manual saves don't hurt
8. **Version Control**: Export your workflow JSON regularly

---

## 🎉 Congratulations!

You've successfully built an AI-powered audio news agent! This workflow demonstrates the power of combining multiple AI services to create engaging, intelligent automation.

**What's Next?**
- Customize the news anchor personality
- Experiment with different voice options
- Add more news categories
- Share your creation with the community!

Happy automating! 🚀

---

**Last Updated**: February 2026
**Version**: 1.0
**Compatible with**: n8n v1.0+, OpenAI API v1+