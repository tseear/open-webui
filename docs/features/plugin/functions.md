# Functions

Functions in Open WebUI are powerful plugins that extend the capabilities of your chat interface. They come in three types: Actions, Filters, and Pipes, each serving a specific purpose in enhancing your chat experience.

## Types of Functions

### 1. Action Functions
- Execute custom actions during chat interactions
- Can perform tasks like sending webhooks or processing data
- Triggered by user interaction
- [Learn more about Action Functions](./functions/action.md)

### 2. Filter Functions
- Process and modify chat messages before they are sent
- Can transform text, add context, or filter content
- Applied automatically to messages
- [Learn more about Filter Functions](./functions/filter.md)

### 3. Pipe Functions
- Process data streams in real-time
- Can transform or analyze data as it flows
- Useful for continuous data processing
- [Learn more about Pipe Functions](./functions/pipe.md)

## Installing Functions

### Method 1: Manual Import

1. Visit the Open WebUI Community site: https://openwebui.com/functions/
2. Select the desired Function
3. Click the "Get" button
4. Choose "Download as JSON export"
5. In Open WebUI, go to Workspace => Functions
6. Click "Import Functions" and upload the downloaded JSON file

### Method 2: Direct Import

1. Visit https://openwebui.com/functions/
2. Select your desired Function
3. Click "Get"
4. Enter your Open WebUI instance URL
5. Click "Import to WebUI"

## Managing Functions

### Enabling/Disabling Functions

1. Navigate to Workspace => Functions
2. Find the function you want to manage
3. Use the toggle switch to enable/disable the function

### Global Functions

Functions can be marked as global, making them available to all models by default:

1. Go to the Functions workspace
2. Find your function
3. Click the globe icon to toggle global status

### Function Settings

Each function can have its own settings (valves):

1. Click the settings icon next to the function
2. Configure the available options
3. Save your changes

## Writing Custom Functions

### Basic Structure

```python
"""
title: Your Function Name
author: Your Name
author_url: https://your-website.com
git_url: https://github.com/your-repo
description: Function description
required_open_webui_version: 0.4.0
requirements: package1>=1.0.0,package2
version: 0.1.0
license: MIT
"""

class YourFunctionType:  # Action, Filter, or Pipe
    def __init__(self):
        """Initialize the Function."""
        self.valves = self.Valves()

    class Valves(BaseModel):
        setting1: str = Field("", description="Setting description")
        setting2: int = Field(0, description="Setting description")

    def your_function(self, param1: str, param2: int) -> str:
        """
        Function description
        :param param1: Parameter description
        :param param2: Parameter description
        :return: Return value description
        """
        # Implementation here
        pass
```

## Best Practices

1. **Code Organization**:
   - Keep functions modular and focused
   - Use clear naming conventions
   - Document your code thoroughly

2. **Error Handling**:
   - Implement comprehensive error handling
   - Provide meaningful error messages
   - Handle edge cases gracefully

3. **Performance**:
   - Optimize for speed
   - Minimize resource usage
   - Use appropriate data structures

4. **Security**:
   - Validate all inputs
   - Handle sensitive data securely
   - Follow security best practices

5. **Testing**:
   - Test thoroughly before deployment
   - Include unit tests
   - Test edge cases

## Requirements

Specify your function's requirements in the frontmatter:

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

1. **Function not working**:
   - Check if the function is enabled
   - Verify all requirements are installed
   - Check the function logs for errors

2. **Configuration issues**:
   - Verify function settings
   - Check for required API keys
   - Ensure proper permissions

3. **Performance problems**:
   - Monitor resource usage
   - Check for bottlenecks
   - Optimize code if needed

## API Reference

### Function Response Format

```json
{
    "status": "success|error",
    "message": "Result message",
    "data": {} // Optional additional data
}
```

### Event System

Functions can use the event system to communicate:

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
- [GitHub Repository](https://github.com/open-webui/open-webui)
- [Discord Community](https://discord.gg/open-webui) 