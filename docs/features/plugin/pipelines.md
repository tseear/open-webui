# Pipelines

Pipelines is a UI-agnostic OpenAI API plugin framework that allows you to extend Open WebUI's capabilities with modular, customizable workflows. It's particularly useful for handling computationally heavy tasks or complex logic that you want to offload from your main Open WebUI instance.

## Overview

Pipelines bring modular, customizable workflows to any UI client supporting OpenAI API specs. It allows you to:
- Add custom logic and integrate Python libraries
- Create AI agents and home automation APIs
- Implement sophisticated RAG pipelines
- Monitor message interactions
- Control request flow and rate limiting
- And much more!

## Quick Start with Docker

```bash
docker run -d -p 9099:9099 \
  --add-host=host.docker.internal:host-gateway \
  -v pipelines:/app/pipelines \
  --name pipelines \
  --restart always \
  ghcr.io/open-webui/pipelines:main
```

### Connecting to Open WebUI

1. Navigate to **Admin Panel > Settings > Connections**
2. Click the "+" button to add a new connection
3. Set the API URL to `http://localhost:9099` (or `http://host.docker.internal:9099` if using Docker)
4. Set the API key to `0p3n-w3bu!`
5. Save the connection

## Types of Pipelines

### 1. Filters
Filters process incoming user messages and outgoing assistant messages. They can:
- Send messages to monitoring platforms
- Modify message contents
- Block toxic messages
- Translate messages
- Rate limit messages

### 2. Pipes
Pipes perform actions before returning LLM messages to users. They can:
- Implement RAG (Retrieval Augmented Generation)
- Connect to non-OpenAI LLM providers
- Execute functions in the web UI
- Process data streams in real-time

## Writing Custom Pipelines

### Basic Structure

```python
"""
title: Your Pipeline Name
author: Your Name
author_url: https://your-website.com
git_url: https://github.com/your-repo
description: Pipeline description
required_open_webui_version: 0.4.0
requirements: package1>=1.0.0,package2
version: 0.1.0
license: MIT
"""

class Pipeline:
    def __init__(self):
        """Initialize the Pipeline."""
        self.valves = self.Valves()

    class Valves(BaseModel):
        setting1: str = Field("", description="Setting description")
        setting2: int = Field(0, description="Setting description")

    async def process(self, data: dict) -> dict:
        """
        Process the input data
        :param data: Input data dictionary
        :return: Processed data dictionary
        """
        # Implementation here
        pass
```

### Valves Configuration

Valves are input variables that can be configured per pipeline:

```python
class Pipeline:
    class Valves(BaseModel):
        # Using environment variables with defaults
        api_key: str = Field(
            default=os.getenv("API_KEY", "default_value"),
            description="API key for external service"
        )
        
        # Optional settings
        max_retries: Optional[int] = Field(
            default=None,
            description="Maximum number of retry attempts"
        )
```

## Directory Structure

```
/pipelines/
├── filters/          # Filter pipeline implementations
├── pipes/           # Pipe pipeline implementations
└── examples/        # Example implementations
```

## Installation and Setup

### Prerequisites
- Python 3.11 (officially supported version)
- Docker (optional, for containerized deployment)

### Manual Installation

1. Clone the repository:
```bash
git clone https://github.com/open-webui/pipelines.git
cd pipelines
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Start the server:
```bash
sh ./start.sh
```

### Docker Installation with Custom Dependencies

```bash
docker run -d -p 9099:9099 \
  --add-host=host.docker.internal:host-gateway \
  -e PIPELINES_URLS="https://github.com/open-webui/pipelines/blob/main/examples/filters/detoxify_filter_pipeline.py" \
  -v pipelines:/app/pipelines \
  --name pipelines \
  --restart always \
  ghcr.io/open-webui/pipelines:main
```

## Best Practices

1. **Error Handling**:
   - Implement comprehensive error handling
   - Provide meaningful error messages
   - Handle edge cases gracefully

2. **Performance**:
   - Optimize for speed
   - Minimize resource usage
   - Use appropriate data structures

3. **Security**:
   - Validate all inputs
   - Handle sensitive data securely
   - Follow security best practices

4. **Documentation**:
   - Document your pipeline thoroughly
   - Include usage examples
   - Explain configuration options

## Troubleshooting

Common issues and solutions:

1. **Connection Issues**:
   - Verify Docker networking
   - Check port availability
   - Ensure proper host configuration

2. **Pipeline Loading**:
   - Check pipeline syntax
   - Verify dependencies
   - Review error logs

3. **Performance Problems**:
   - Monitor resource usage
   - Check for bottlenecks
   - Optimize code if needed

## API Reference

### Pipeline Response Format

```json
{
    "status": "success|error",
    "message": "Result message",
    "data": {} // Optional additional data
}
```

### Event System

Pipelines can use the event system to communicate:

```python
# Emit an event
await __event_emitter__({
    "type": "event_type",
    "data": {
        "key": "value"
    }
})

# Handle an event
response = await __event_call__({
    "type": "event_type",
    "data": {
        "key": "value"
    }
})
```

## Community Resources

- [Open WebUI Community](https://openwebui.com)
- [GitHub Repository](https://github.com/open-webui/pipelines)
- [Discord Community](https://discord.gg/open-webui)
- [Pipeline Examples](https://github.com/open-webui/pipelines/tree/main/examples) 