# Pipes

Pipes are a type of pipeline that perform actions before returning LLM messages to users. They can implement complex workflows, integrate with external services, and process data streams in real-time.

## Overview

Pipes are designed to:
- Implement RAG (Retrieval Augmented Generation)
- Connect to non-OpenAI LLM providers
- Execute functions in the web UI
- Process data streams in real-time
- Transform or analyze data

## Basic Structure

```python
"""
title: Your Pipe Name
author: Your Name
author_url: https://your-website.com
git_url: https://github.com/your-repo
description: Pipe description
required_open_webui_version: 0.4.0
requirements: package1>=1.0.0,package2
version: 0.1.0
license: MIT
"""

class Pipe:
    def __init__(self):
        """Initialize the Pipe."""
        self.valves = self.Valves()

    class Valves(BaseModel):
        api_key: str = Field("", description="API key for external service")
        max_tokens: int = Field(1000, description="Maximum tokens to generate")

    async def pipe(self, data: dict) -> dict:
        """
        Process the input data
        :param data: Input data dictionary
        :return: Processed data dictionary
        """
        # Implementation here
        pass
```

## Example: RAG Pipe

Here's an example of a pipe that implements RAG:

```python
from langchain.vectorstores import FAISS
from langchain.embeddings import OpenAIEmbeddings

class Pipe:
    def __init__(self):
        self.valves = self.Valves()
        self.embeddings = OpenAIEmbeddings(openai_api_key=self.valves.api_key)
        self.vectorstore = FAISS.load_local(
            "path/to/vectorstore",
            self.embeddings
        )

    class Valves(BaseModel):
        api_key: str = Field("", description="OpenAI API key")
        k: int = Field(3, description="Number of documents to retrieve")

    async def pipe(self, data: dict) -> dict:
        messages = data.get('messages', [])
        if not messages:
            return data

        query = messages[-1].get('content', '')
        if not query:
            return data

        # Retrieve relevant documents
        docs = self.vectorstore.similarity_search(query, k=self.valves.k)
        
        # Add context to the prompt
        context = "\n".join([doc.page_content for doc in docs])
        messages[-1]['content'] = f"Context:\n{context}\n\nQuestion: {query}"

        return {'messages': messages}
```

## Common Use Cases

### 1. RAG Implementation
- Retrieve relevant documents
- Add context to prompts
- Generate informed responses

### 2. External LLM Integration
- Connect to other LLM providers
- Implement custom models
- Handle different API formats

### 3. Data Processing
- Transform data formats
- Analyze content
- Generate summaries

### 4. Function Execution
- Call external APIs
- Process files
- Execute system commands

## Best Practices

1. **Performance**:
   - Optimize document retrieval
   - Use efficient data structures
   - Implement caching

2. **Error Handling**:
   - Handle API failures gracefully
   - Provide fallback options
   - Log important events

3. **Configuration**:
   - Make pipes configurable
   - Use environment variables
   - Document all options

## Troubleshooting

Common issues and solutions:

1. **Pipe Not Working**:
   - Check API keys
   - Verify dependencies
   - Review error logs

2. **Performance Issues**:
   - Optimize retrieval
   - Check resource usage
   - Consider caching

3. **Integration Problems**:
   - Verify API endpoints
   - Check authentication
   - Test connectivity

## API Reference

### Pipe Response Format

```json
{
    "messages": [
        {
            "content": "Processed message content",
            "role": "assistant"
        }
    ]
}
```

Or for error cases:

```json
{
    "error": {
        "message": "Error description",
        "type": "pipe_error"
    }
}
```

### Event System

Pipes can use the event system to communicate:

```python
# Emit a status update
await __event_emitter__({
    "type": "status",
    "data": {
        "description": "Processing documents...",
        "done": False
    }
})

# Emit a message
await __event_emitter__({
    "type": "message",
    "data": {
        "content": "Documents processed successfully"
    }
})
``` 