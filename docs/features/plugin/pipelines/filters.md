# Filters

Filters are a type of pipeline that process incoming user messages and outgoing assistant messages. They can modify, block, or enhance messages based on various criteria.

## Overview

Filters are designed to:
- Process messages before they reach the LLM
- Modify or block messages based on content
- Add context or metadata to messages
- Implement rate limiting
- Send messages to monitoring platforms

## Basic Structure

```python
"""
title: Your Filter Name
author: Your Name
author_url: https://your-website.com
git_url: https://github.com/your-repo
description: Filter description
required_open_webui_version: 0.4.0
requirements: package1>=1.0.0,package2
version: 0.1.0
license: MIT
"""

class Filter:
    def __init__(self):
        """Initialize the Filter."""
        self.valves = self.Valves()

    class Valves(BaseModel):
        threshold: float = Field(0.5, description="Toxicity threshold")
        max_length: int = Field(1000, description="Maximum message length")

    async def filter(self, data: dict) -> dict:
        """
        Filter the input data
        :param data: Input data dictionary containing message content
        :return: Filtered data dictionary
        """
        # Implementation here
        pass
```

## Example: Toxicity Filter

Here's an example of a filter that checks for toxic content:

```python
from detoxify import Detoxify

class Filter:
    def __init__(self):
        self.valves = self.Valves()
        self.model = Detoxify('multilingual')

    class Valves(BaseModel):
        threshold: float = Field(0.5, description="Toxicity threshold")

    async def filter(self, data: dict) -> dict:
        message = data.get('messages', [{}])[-1].get('content', '')
        
        if not message:
            return data

        results = self.model.predict(message)
        toxicity_score = results['toxicity']

        if toxicity_score > self.valves.threshold:
            return {
                'error': {
                    'message': 'Message contains toxic content',
                    'type': 'filtered'
                }
            }

        return data
```

## Common Use Cases

### 1. Content Moderation
- Filter out inappropriate content
- Block spam messages
- Remove sensitive information

### 2. Message Enhancement
- Add context to messages
- Translate messages
- Format messages

### 3. Rate Limiting
- Limit message frequency
- Track usage patterns
- Implement quotas

### 4. Monitoring
- Log messages
- Send alerts
- Track metrics

## Best Practices

1. **Performance**:
   - Keep filters lightweight
   - Use efficient algorithms
   - Cache results when possible

2. **Error Handling**:
   - Handle edge cases gracefully
   - Provide meaningful error messages
   - Log important events

3. **Configuration**:
   - Make filters configurable
   - Use sensible defaults
   - Document all options

## Troubleshooting

Common issues and solutions:

1. **Filter Not Working**:
   - Check filter configuration
   - Verify dependencies
   - Review error logs

2. **Performance Issues**:
   - Optimize processing
   - Check resource usage
   - Consider caching

3. **False Positives/Negatives**:
   - Adjust thresholds
   - Review filtering logic
   - Test with various inputs

## API Reference

### Filter Response Format

```json
{
    "error": {
        "message": "Error description",
        "type": "filtered"
    }
}
```

Or for successful filtering:

```json
{
    "messages": [
        {
            "content": "Filtered message content",
            "role": "user"
        }
    ]
}
``` 