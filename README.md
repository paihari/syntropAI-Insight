# syntropAI-Insight

A conversational AI agent that represents Hariprasad Bantwal, providing an interactive chat interface for website visitors to learn about his career, background, skills, and experience.

## What This Does

This project creates an AI-powered chat interface that:

- **Acts as a Digital Representative**: The AI agent impersonates Hariprasad Bantwal, answering questions about his professional background and experience
- **Provides Career Information**: Shares details about skills, experience, and background based on LinkedIn profile and summary data
- **Engages Website Visitors**: Offers a conversational way for potential clients or employers to learn about Hariprasad
- **Captures Leads**: Records visitor information and unanswered questions for follow-up
- **PDF Processing**: Reads and extracts information from LinkedIn PDF profiles
- **Real-time Notifications**: Sends push notifications when new interactions occur

## Features

- **Gradio Chat Interface**: Modern, user-friendly web-based chat UI
- **PDF Document Processing**: Automatically extracts text from LinkedIn PDF profiles
- **Tool-based Functionality**: Uses OpenAI function calling for structured data collection
- **Push Notifications**: Real-time alerts via Pushover for new interactions
- **Environment-based Configuration**: Secure API key and token management

## Agent Design

### Core Architecture

The agent is built around a `Me` class that encapsulates the personality and capabilities of Hariprasad Bantwal:

```python
class Me:
    def __init__(self):
        # Initialize OpenAI client
        # Load LinkedIn profile from PDF
        # Load personal summary
```

### Simple Design in Layers

```
┌─────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                       │
├─────────────────────────────────────────────────────────────┤
│  Gradio Web Interface                                       │
│  • Chat UI                                                  │
│  • User interaction handling                                │
└─────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER                        │
├─────────────────────────────────────────────────────────────┤
│  Me Class (Agent Logic)                                     │
│  • System prompt management                                 │
│  • Conversation flow control                                │
│  • Tool execution coordination                              │
└─────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────┐
│                    SERVICE LAYER                            │
├─────────────────────────────────────────────────────────────┤
│  External Services                                          │
│  • OpenAI GPT-4o-mini API                                  │
│  • Pushover Notification API                                │
└─────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────┐
│                    DATA LAYER                               │
├─────────────────────────────────────────────────────────────┤
│  Knowledge Base                                             │
│  • LinkedIn PDF (./me/linkedin.pdf)                        │
│  • Personal Summary (./me/summary.txt)                     │
│  • Environment Variables (.env)                            │
└─────────────────────────────────────────────────────────────┘
```

### Knowledge Base

The agent's knowledge comes from two primary sources:
1. **LinkedIn Profile PDF**: Automatically extracted text from `./me/linkedin.pdf`
2. **Personal Summary**: Curated background information from `./me/summary.txt`

### System Prompt Design

The agent uses a carefully crafted system prompt that:
- Establishes the AI's role as Hariprasad Bantwal
- Provides context about the website interaction purpose
- Includes the knowledge base content
- Sets professional and engaging communication guidelines

### Tool Integration

The agent has access to two specialized tools:

#### 1. User Details Recording (`record_user_details`)
- **Purpose**: Capture interested visitors' contact information
- **Parameters**: email, name, notes
- **Trigger**: When users show interest in getting in touch

#### 2. Unknown Question Recording (`record_unknown_question`)
- **Purpose**: Track questions the agent cannot answer
- **Parameters**: question
- **Trigger**: When the agent encounters questions outside its knowledge scope

### Conversation Flow

1. **Initialization**: Agent loads profile data and establishes system context
2. **Message Processing**: Each user message is processed through the OpenAI API
3. **Tool Execution**: If function calls are needed, tools are executed and results returned
4. **Response Generation**: Final response is generated and displayed to the user
5. **Notification**: Relevant interactions trigger push notifications

### Error Handling

- **Unknown Questions**: Automatically recorded for human review
- **Tool Failures**: Graceful degradation with fallback responses
- **API Issues**: Robust error handling for OpenAI API calls

## Setup

1. Install dependencies:
   ```bash
   uv pip install openai gradio PyPDF2 python-dotenv requests
   ```

2. Create a `.env` file with:
   ```
   OPENAI_API_KEY=your_openai_api_key
   PUSHOVER_TOKEN=your_pushover_token
   PUSHOVER_USER=your_pushover_user
   ```

3. Prepare your data:
   - Place LinkedIn profile PDF at `./me/linkedin.pdf`
   - Create summary file at `./me/summary.txt`

4. Run the application:
   ```bash
   python src/main.py
   ```

## Usage

The application launches a Gradio web interface where visitors can:
- Ask questions about Hariprasad's background
- Learn about his skills and experience
- Get in touch by providing their contact information
- Have their questions answered in real-time

## Technical Stack

- **Python**: Core application logic
- **OpenAI GPT-4o-mini**: LLM for conversation generation
- **Gradio**: Web interface framework
- **PyPDF2**: PDF text extraction
- **Pushover**: Push notification service
- **python-dotenv**: Environment variable management