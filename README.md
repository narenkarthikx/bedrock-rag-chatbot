# RAG Chatbot with Amazon Bedrock and ChromaDB

A **Retrieval-Augmented Generation (RAG)** chatbot built with Amazon Bedrock, ChromaDB, and Streamlit that provides interactive conversations enhanced by specialized knowledge from a vector database.

## 🏗️ Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Streamlit     │    │   RAG Library    │    │  Amazon Bedrock │
│   Frontend      │◄──►│                  │◄──►│   Claude 3      │
│                 │    │                  │    │                 │
└─────────────────┘    └──────┬───────────┘    └─────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │   ChromaDB      │
                    │ Vector Database │
                    │                 │
                    └─────────────────┘
```

### How it Works

1. **Chat Memory**: Past interactions are tracked in the chat memory object
2. **User Input**: The user enters a new message through the Streamlit interface
3. **Context Building**: Chat history is retrieved and combined with the new message
4. **Vector Search**: The question is converted to embeddings using Amazon Titan, then matched to closest vectors in ChromaDB
5. **RAG Processing**: Combined history, retrieved knowledge, and new message are sent to Claude 3
6. **Response**: The model's enhanced response is displayed to the user

## 🚀 Features

- **Interactive Chat Interface**: Clean Streamlit-based UI for seamless conversations
- **Vector-Enhanced Responses**: Leverages ChromaDB for contextual information retrieval
- **Tool-Based Architecture**: Uses Amazon Bedrock's tool calling capabilities for structured RAG
- **Session Memory**: Maintains conversation context with configurable message history (max 20 messages)
- **Amazon Bedrock Integration**: Powered by Claude 3 Sonnet model for high-quality responses

## 📁 Project Structure

```
rag_chatbot/
├── rag_chatbot_app.py      # Streamlit frontend application
├── rag_chatbot_lib.py      # Core RAG functionality and Bedrock integration
└── __pycache__/            # Python cache directory
```

## 🛠️ Prerequisites

### AWS Setup
- AWS Account with appropriate permissions
- Amazon Bedrock access enabled
- AWS credentials configured (via AWS CLI, environment variables, or IAM roles)

### Required Models
- **Claude 3 Sonnet**: `us.anthropic.claude-3-7-sonnet-20250219-v1:0`
- **Titan Embeddings**: `amazon.titan-embed-text-v2:0`

### Python Dependencies
```bash
pip install streamlit boto3 chromadb
```

## ⚙️ Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/narenkarthikx/bedrock-rag-chatbot.git
cd bedrock-rag-chatbot
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Configure AWS Credentials
```bash
# Option 1: AWS CLI
aws configure

# Option 2: Environment Variables
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_DEFAULT_REGION=your_region
```

### 4. Set Up Vector Database
```bash
# Navigate to your data directory
cd /path/to/your/data

# Initialize the ChromaDB collection
python3 populate_collection.py
```

### 5. Update Database Path
Modify the path in `rag_chatbot_lib.py` to match your ChromaDB location:
```python
collection = get_collection("../../data/chroma", "bedrock_faqs_collection")
```

## 🚀 Running the Application

### Start the Streamlit App
```bash
cd rag_chatbot
streamlit run rag_chatbot_app.py
```

The application will be available at `http://localhost:8501`

## 📋 Core Components

### `rag_chatbot_app.py`
- **Streamlit Frontend**: Handles user interface and chat interactions
- **Session Management**: Maintains chat history across interactions
- **Message Rendering**: Displays conversation history with proper formatting

### `rag_chatbot_lib.py`
- **ChatMessage Class**: Stores chat messages with role and text
- **Vector Operations**: Connects to ChromaDB and performs similarity searches
- **Bedrock Integration**: Handles API calls to Amazon Bedrock models
- **Tool Processing**: Manages RAG tool calls and response processing
- **Conversation Management**: Maintains message history and formats API requests

## 🔧 Key Functions

| Function | Purpose |
|----------|---------|
| `get_collection()` | Connects to ChromaDB with Bedrock embeddings |
| `get_vector_search_results()` | Performs similarity search in vector database |
| `chat_with_model()` | Main conversation handler with RAG capabilities |
| `process_tool()` | Handles tool use requests and RAG processing |
| `convert_chat_messages_to_converse_api()` | Formats messages for Bedrock API |

## 🎛️ Configuration

### Model Settings
```python
MODEL_ID = "us.anthropic.claude-3-7-sonnet-20250219-v1:0"
EMBEDDING_MODEL = "amazon.titan-embed-text-v2:0"
MAX_MESSAGES = 20  # Maximum chat history length
```

### Inference Parameters
```python
inferenceConfig = {
    "maxTokens": 2000,
    "temperature": 0,
    "topP": 0.9,
    "stopSequences": []
}
```

## 🔍 Use Cases

This RAG chatbot is ideal for:

- **Customer Support**: Enhanced with company knowledge base
- **Technical Documentation**: Interactive querying of technical documents
- **Educational Content**: Learning assistance with course materials
- **FAQ Systems**: Intelligent question answering with context
- **Knowledge Management**: Organizational information retrieval

## 🐛 Troubleshooting

### Common Issues

1. **AWS Credentials Error**
   ```
   Solution: Ensure AWS credentials are properly configured
   ```

2. **ChromaDB Connection Failed**
   ```
   Solution: Verify the database path and ensure collection exists
   ```

3. **Model Access Denied**
   ```
   Solution: Request access to Claude 3 Sonnet in Amazon Bedrock console
   ```

4. **Streamlit Port Issues**
   ```bash
   # Use different port
   streamlit run rag_chatbot_app.py --server.port 8502
   ```

## 📈 Performance Optimization

- **Vector Search**: Adjust `n_results` parameter for retrieval balance
- **Memory Management**: Tune `MAX_MESSAGES` based on use case
- **Embedding Caching**: Implement caching for frequently accessed vectors
- **Parallel Processing**: Consider async operations for better response times

## 🛡️ Security Considerations

- Store AWS credentials securely (avoid hardcoding)
- Implement proper access controls for sensitive data
- Validate user inputs to prevent injection attacks
- Monitor and log API usage for security auditing


**Built with ❤️ using Amazon Bedrock, ChromaDB, and Streamlit**