# Tools

Tools in Open WebUI are Python scripts that extend the capabilities of Large Language Models (LLMs) by providing them with real-world, real-time data and functionality. They enable LLMs to perform actions and receive additional context during conversations.

## Overview

Tools are designed to enhance LLM interactions by providing external abilities that LLMs wouldn't otherwise have access to. They can fetch real-time data, interact with external APIs, and perform specialized tasks based on conversation context.

## Key Features

- Real-time data retrieval (weather, stock prices, etc.)
- External API integrations
- Web search capabilities
- Image generation
- Voice synthesis
- Custom data processing

## Installing Tools

### Method 1: Manual Import

1. Visit the Open WebUI Community site: https://openwebui.com/tools/
2. Select the desired Tool
3. Click the "Get" button
4. Choose "Download as JSON export"
5. In Open WebUI, go to Workspace => Tools
6. Click "Import Tools" and upload the downloaded JSON file

### Method 2: Direct Import

1. Visit https://openwebui.com/tools/
2. Select your desired Tool
3. Click "Get"
4. Enter your Open WebUI instance URL
5. Click "Import to WebUI"

## Using Tools

1. Navigate to Workspace => Models
2. Select your model
3. Click the edit (pencil) icon
4. Scroll to the Tools section
5. Enable desired Tools
6. Save changes

Tools can be accessed during chat by:
- Clicking the "+" icon in the chat interface
- Using the AutoTool Filter (available at https://openwebui.com/f/hub/autotool_filter/)

## Writing Custom Tools

### Basic Structure

```python
"""
title: Your Tool Name
author: Your Name
author_url: https://your-website.com
git_url: https://github.com/your-repo
description: Tool description
required_open_webui_version: 0.4.0
requirements: package1>=1.0.0,package2
version: 0.1.0
license: MIT
"""

class Tools:
    def __init__(self):
        """Initialize the Tool."""
        self.valves = self.Valves()

    class Valves(BaseModel):
        api_key: str = Field("", description="Your API key here")

    def your_tool_function(self, param1: str, param2: int) -> str:
        """
        Tool function description
        :param param1: Parameter description
        :param param2: Parameter description
        :return: Return value description
        """
        # Implementation here
        pass
```

### Event Emitters

Tools can emit different types of events:

1. **Status Updates**:
```python
await __event_emitter__({
    "type": "status",
    "data": {
        "description": "Processing...",
        "done": False
    }
})
```

2. **Messages**:
```python
await __event_emitter__({
    "type": "message",
    "data": {
        "content": "Your message here"
    }
})
```

3. **Citations**:
```python
await __event_emitter__({
    "type": "citation",
    "data": {
        "document": ["Content here"],
        "metadata": [{
            "date_accessed": datetime.now().isoformat(),
            "source": "Source name"
        }],
        "source": {
            "name": "Source name",
            "url": "https://source-url.com"
        }
    }
})
```

## Best Practices

1. **Error Handling**: Implement comprehensive error handling
2. **Input Validation**: Validate all inputs before processing
3. **Rate Limiting**: Implement appropriate rate limiting for API calls
4. **Documentation**: Provide clear documentation for your tool
5. **Security**: Handle sensitive data (like API keys) securely
6. **Performance**: Optimize for speed and efficiency

## Requirements

Specify your tool's requirements in the frontmatter:

```yaml
---
required_open_webui_version: "0.4.0"
requirements:
  - requests>=2.25.0
  - python-dateutil>=2.8.2
---
```

## Troubleshooting

Common issues and solutions:

1. **Tool not appearing**:
   - Verify the tool is properly imported
   - Check if it's enabled for your model
   - Ensure all requirements are installed

2. **API errors**:
   - Verify API keys are correctly set
   - Check API rate limits
   - Ensure network connectivity

3. **Performance issues**:
   - Check for resource-intensive operations
   - Verify proper error handling
   - Monitor API response times

## API Reference

### Tool Response Format

```json
{
    "status": "success|error",
    "message": "Result message",
    "data": {} // Optional additional data
}
``` 