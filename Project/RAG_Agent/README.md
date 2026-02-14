# Building a RAG Agent with n8n - Complete Guide

## 📚 Overview

This guide will teach you how to build a **Retrieval-Augmented Generation (RAG) Agent** using n8n. By the end of this tutorial, you'll have created an intelligent chatbot that can answer questions based on documents stored in a vector database.

**What you'll build:**
- An automated document ingestion pipeline (Part 1)
- An AI chatbot that retrieves and answers questions from your documents (Part 2)

**Use Case Example:** An iOS 18 feature assistant that answers questions about new features, settings, and updates based on uploaded PDF documentation.

---

## 🎯 Prerequisites

Before starting, ensure you have:

1. **n8n Account** - [Sign up here](https://n8n.io/)
2. **Google Drive Account** - For document storage
3. **OpenAI API Key** - [Get it here](https://platform.openai.com/)
4. **Pinecone Account** - [Sign up here](https://www.pinecone.io/)

### Required API Keys & Credentials:
- OpenAI API Key
- Pinecone API Key
- Google Drive OAuth credentials

---

## 🏗️ Architecture Overview

This workflow consists of **two main parts**:

### Part 1: Document Ingestion Pipeline
```
Google Drive (New File) → Download → Process → Split → Embed → Store in Pinecone
```

### Part 2: Chat Interface
```
User Question → AI Agent → Vector Search → Retrieve Context → Generate Answer
```

---

## 📋 Part 1: Setting Up Document Ingestion

### Step 1: Create a New Workflow

1. Log in to your n8n account
2. Click **"New Workflow"**
3. Name it: `Overall_RAG_Agent`

---

### Step 2: Add Google Drive Trigger

1. **Add Node**: Click the `+` button and search for **"Google Drive Trigger"**
2. **Configure the trigger**:
   - **Trigger On**: `Specific Folder`
   - **Event**: `File Created`
   - **Poll Times**: `Every Minute`
   
3. **Select Folder**:
   - Click **"Select a Folder"**
   - Authenticate with your Google account
   - Create or select a folder (e.g., "First RAG agent")
   - Note: This folder will be monitored for new PDF uploads

4. **Test**: Upload a PDF to verify the trigger works

---

### Step 3: Download File from Google Drive

1. **Add Node**: Connect **"Google Drive"** node to the trigger
2. **Configure**:
   - **Operation**: `Download`
   - **File ID**: Use expression `{{ $json.id }}`
   
   This expression captures the file ID from the trigger output.

3. **What this does**: Downloads the file content in binary format for processing

---

### Step 4: Set Up Document Loader

1. **Add Node**: **"Default Data Loader"** (search for "document loader")
2. **Configure**:
   - **Data Type**: `Binary`
   - **Loader**: `PDF Loader`
   
3. **Add Metadata** (Optional but recommended):
   - Click **"Add Option"** → **"Metadata"**
   - Add metadata value:
     - **Name**: `filename`
     - **Value**: `={{ $json.name }}`
   
   This helps track which document each chunk came from.

---

### Step 5: Configure Text Splitter

1. **Add Node**: **"Recursive Character Text Splitter"**
2. **Connect to**: Default Data Loader (via the purple AI connection point)
3. **Configure**:
   - **Chunk Size**: `900` characters
   - **Chunk Overlap**: `50` characters

**Why these settings?**
- **Chunk Size (900)**: Balances context preservation with processing efficiency
- **Overlap (50)**: Prevents information loss at chunk boundaries

---

### Step 6: Set Up OpenAI Embeddings

1. **Add Node**: **"Embeddings OpenAI"**
2. **Configure**:
   - **Connection**: Add your OpenAI API credentials
   - **Model**: Default (text-embedding-ada-002)
   
3. **What this does**: Converts text chunks into numerical vectors for similarity search

---

### Step 7: Configure Pinecone Vector Store

1. **Add Node**: **"Pinecone Vector Store"**
2. **Configure**:
   - **Mode**: `Insert`
   - **Pinecone Index**: Select or create `n8nragagent`
   - **Namespace**: `ragagent`

3. **Connect the following nodes to Pinecone**:
   - Default Data Loader → **Document Input** (purple)
   - Embeddings OpenAI → **Embedding Input** (purple)
   - Download File → **Main Input** (gray)

**What is a namespace?**
A namespace is like a folder within your Pinecone index, allowing you to organize different document collections.

---

### Step 8: Test the Ingestion Pipeline

1. **Activate the workflow**: Toggle the switch in the top right
2. **Upload a test PDF** to your Google Drive folder
3. **Check execution**: 
   - Go to **"Executions"** tab
   - Verify all nodes executed successfully
   - Check Pinecone dashboard to confirm vectors were stored

---

## 💬 Part 2: Building the Chat Interface

### Step 9: Add Chat Trigger

1. **Add Node**: **"When chat message received"**
2. **Configure**:
   - **Make Chat Publicly Available**: ✅ Enable
   - This creates a public chat interface URL

3. **Save** and note the webhook URL generated

---

### Step 10: Set Up AI Agent

1. **Add Node**: **"AI Agent"**
2. **Connect to**: Chat Trigger
3. **Configure System Message**:

```
Role
You are an AI assistant trained to answer questions specifically based on the iOS 18 feature documents stored in the connected vector database.

Objective
Your job is to help users clearly understand the new features, settings, and changes introduced in iOS 18 – by retrieving and referencing only the information available in the documents stored in the vector store.

If a user asks something that's not included in the uploaded content, or if the answer isn't clear from the available data, let them know that the information isn't currently available.

Rules
- Be clear and direct – avoid over-explaining. (Use 2-5 sentences)
- Use bullet points or short paragraphs when listing multiple features.
- Never guess or assume – stick strictly to what's inside the vector database content.

Your main objective: Deliver helpful, accurate, and easy-to-understand answers using only the documents available in the vector store – no speculation, no extra assumptions, and no external sources.
```

**Customize this prompt** for your specific use case!

---

### Step 11: Add OpenAI Chat Model (for Agent)

1. **Add Node**: **"OpenAI Chat Model"**
2. **Configure**:
   - **Model**: `gpt-4o` (or gpt-3.5-turbo for cost savings)
   - **Connection**: Use your OpenAI credentials

3. **Connect to**: AI Agent (purple connection - Language Model input)

---

### Step 12: Add Memory Component

1. **Add Node**: **"Window Buffer Memory"**
2. **Configure**:
   - **Context Window Length**: `10` messages

3. **Connect to**: AI Agent (purple connection - Memory input)

**What this does**: Remembers the last 10 messages for context-aware conversations.

---

### Step 13: Create Vector Store Retrieval Tool

1. **Add Node**: **"Vector Store Tool"**
2. **Name it**: `IOS_18` (or relevant to your use case)
3. **Configure**:
   - **Name**: `IOS_18`
   - **Description**: 
   ```
   Useful for when you need to answer questions about iOS 18. It finds and returns the most relevant information from the documents in the vector database – like feature updates, settings, or changes in the new iOS version. Whenever you need information about iOS 18, you should ALWAYS use this.
   ```

4. **Connect to**: AI Agent (purple connection - Tool input)

---

### Step 14: Add Pinecone Vector Store (for Retrieval)

1. **Add Node**: **"Pinecone Vector Store"** (second instance)
2. **Configure**:
   - **Mode**: Default (Retrieve)
   - **Pinecone Index**: `n8nragagent` (same as ingestion)
   - **Namespace**: `ragagent` (same as ingestion)

3. **Connect to**: Vector Store Tool (purple connection)

---

### Step 15: Add OpenAI Embeddings (for Queries)

1. **Add Node**: **"Embeddings OpenAI"** (second instance)
2. **Configure**: Use same settings as before
3. **Connect to**: Pinecone Vector Store (the retrieval one)

---

### Step 16: Add OpenAI Chat Model (for Tool)

1. **Add Node**: **"OpenAI Chat Model"** (third instance)
2. **Configure**:
   - **Model**: `gpt-4o`
3. **Connect to**: Vector Store Tool (purple connection - Language Model input)

---

### Step 17: Test Your Chatbot

1. **Save the workflow**
2. **Activate it**
3. **Open the chat URL** (from the Chat Trigger node)
4. **Ask questions** like:
   - "What are the new features in iOS 18?"
   - "How do I customize the Control Center?"
   - "Tell me about the new Photos app features"

---

## 🎨 Visual Layout Tips

### Recommended Node Placement:

**Left Section (Ingestion Pipeline):**
```
Google Drive Trigger
    ↓
Download File
    ↓
Pinecone Vector Store
    ← Default Data Loader ← Text Splitter
    ← Embeddings OpenAI
```

**Right Section (Chat Interface):**
```
Chat Trigger
    ↓
AI Agent
    ← OpenAI Chat Model
    ← Window Buffer Memory
    ← Vector Store Tool
        ← Pinecone Vector Store ← Embeddings OpenAI
        ← OpenAI Chat Model
```

---

## 🔧 Common Issues & Troubleshooting

### Issue 1: "Pinecone index not found"
**Solution**: 
1. Go to Pinecone dashboard
2. Create index named `n8nragagent`
3. Dimension: 1536 (for OpenAI embeddings)
4. Metric: Cosine

### Issue 2: "No vectors returned from search"
**Solution**:
1. Check namespace spelling matches exactly
2. Verify documents were successfully uploaded
3. Check Pinecone dashboard for vector count

### Issue 3: "Agent not using the tool"
**Solution**:
1. Review tool description - make it clear and specific
2. Add phrases like "ALWAYS use this tool when..."
3. Test with direct questions related to your documents

### Issue 4: "API rate limit exceeded"
**Solution**:
1. Add delays between requests
2. Reduce chunk size
3. Upgrade your OpenAI plan

---

## 📊 Understanding the Components

### What is RAG?
**Retrieval-Augmented Generation** combines:
- **Retrieval**: Finding relevant information from a database
- **Generation**: Using AI to create natural language responses

### Vector Databases Explained
- **Vectors**: Numerical representations of text
- **Embeddings**: The process of converting text to vectors
- **Similarity Search**: Finding vectors close to your query vector

### Why Pinecone?
- Fast similarity search
- Scalable to millions of vectors
- Managed service (no infrastructure needed)

---

## 🚀 Advanced Customization

### 1. Multiple Document Types
Add different loaders:
- CSV Loader
- JSON Loader
- Text Loader

### 2. Enhanced Metadata
Add more metadata fields:
- Upload date
- Document category
- Document version

### 3. Better Chunking Strategy
Experiment with:
- Different chunk sizes (500-2000)
- Semantic chunking
- Markdown-aware splitting

### 4. Multiple Namespaces
Organize by:
- Document type
- Topic
- Date

### 5. Improved System Prompts
Add:
- Citation requirements
- Formatting preferences
- Response length constraints

---

## 📈 Best Practices

### 1. Document Preparation
- Clean PDFs (remove scanned images if possible)
- Use text-based PDFs
- Organize documents logically

### 2. Chunk Size Selection
- **Small chunks (300-500)**: Better precision, may lose context
- **Large chunks (1000-2000)**: Better context, may reduce precision
- **Sweet spot**: 800-1200 for most use cases

### 3. System Prompt Design
- Be specific about data source
- Set clear boundaries
- Define response format
- Include examples if needed

### 4. Testing
- Test with various question types
- Check for hallucinations
- Verify source attribution

---

## 💰 Cost Optimization

### OpenAI Costs
- **Embeddings**: ~$0.0001 per 1K tokens
- **GPT-4**: ~$0.03 per 1K tokens (input)
- **GPT-3.5-Turbo**: ~$0.0015 per 1K tokens

### Pinecone Costs
- Free tier: 1 index, 100K vectors
- Paid: Starts at $70/month

### Tips to Reduce Costs
1. Use GPT-3.5-Turbo for testing
2. Optimize chunk size
3. Cache frequent queries
4. Use smaller context windows

---

## 🎓 Learning Exercises

### Exercise 1: Change the Domain
Replace iOS 18 documents with:
- Company policies
- Product documentation
- Course materials

### Exercise 2: Add Multiple Tools
Create tools for:
- Different document sets
- Web search
- Calculator functions

### Exercise 3: Improve Retrieval
Experiment with:
- Different embedding models
- Metadata filters
- Hybrid search (keyword + vector)

### Exercise 4: Add Features
Implement:
- Source citation
- Confidence scores
- Multi-language support

---

## 📚 Additional Resources

### Documentation
- [n8n Documentation](https://docs.n8n.io/)
- [OpenAI API Reference](https://platform.openai.com/docs)
- [Pinecone Documentation](https://docs.pinecone.io/)

### Learning Materials
- [RAG Fundamentals](https://www.pinecone.io/learn/retrieval-augmented-generation/)
- [Vector Database Guide](https://www.pinecone.io/learn/vector-database/)
- [Prompt Engineering](https://platform.openai.com/docs/guides/prompt-engineering)

---

## 🤝 Contributing

Found an issue or want to improve this guide?
- Open an issue on GitHub
- Submit a pull request
- Share your use case!

---

## 📝 License

This guide is open source and available under the MIT License.

---

## ✅ Checklist

Before deploying to production:

- [ ] Test with various document types
- [ ] Verify all API keys are secure
- [ ] Test error handling
- [ ] Set up monitoring
- [ ] Document your specific use case
- [ ] Train users on limitations
- [ ] Plan for scaling
- [ ] Set up backup procedures

---

## 🎉 Conclusion

Congratulations! You've built a fully functional RAG agent. This system can:
- Automatically ingest documents
- Create searchable vector embeddings
- Answer questions based on your documents
- Maintain conversation context
- Scale to thousands of documents

### Next Steps:
1. Customize for your specific use case
2. Add more documents
3. Refine the system prompt
4. Share with users and gather feedback

Happy building! 🚀

---

**Created by**: Ashu Mishra  
**GitHub**: [@ashumishra](https://github.com/ashumish/)  
**LinkedIn**: [linkedin.com/in/ashumish](https://linkedin.com/in/ashumish/)

*For questions or support, feel free to reach out!*