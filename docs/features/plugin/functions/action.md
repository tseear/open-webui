# Action Function

Action Functions in Open WebUI are powerful plugins that allow you to extend the functionality of your chat interface by adding custom actions that can be triggered during conversations. These actions can perform various tasks like sending webhooks, processing data, or integrating with external services.

## Overview

Action Functions are one of three types of functions in Open WebUI (along with Filter and Pipe functions). They are specifically designed to execute custom actions when triggered during chat interactions.

## Creating an Action Function

To create a new Action Function:

1. Navigate to the Functions workspace in the admin panel
2. Click the "+" button to create a new function
3. Select "Action" as the function type
4. Fill in the required information:
   - ID: A unique identifier for your function
   - Name: A display name for your function
   - Description: A brief description of what your function does
   - Content: The Python code that implements your action

## Action Function Structure

An Action Function must implement an `Action` class with an `action` method. Here's the basic structure:

```python
class Action:
    def action(self, body, **kwargs):
        # Your action implementation here
        pass
```

### Available Parameters

The `action` method receives the following parameters:

- `body`: The request body containing:
  - `model`: The model ID being used
  - `chat_id`: The current chat ID
  - `message_id`: The current message ID
  - `session_id`: The current session ID
  - Any additional form data passed to the action

- `__model__`: The model configuration object
- `__id__`: The action ID
- `__event_emitter__`: Event emitter for sending events
- `__event_call__`: Event caller for handling events
- `__request__`: The FastAPI request object
- `__user__`: User information (if available):
  - `id`: User ID
  - `email`: User email
  - `name`: User name
  - `role`: User role
  - `valves`: User-specific configuration (if defined)

## Example Action Function

Here's an example of a simple webhook action that sends a notification when triggered:

```python
import requests

class Action:
    def action(self, body, **kwargs):
        webhook_url = "https://your-webhook-url.com"
        message = f"Action triggered in chat {body['chat_id']}"
        
        try:
            response = requests.post(webhook_url, json={"text": message})
            response.raise_for_status()
            return {"status": "success", "message": "Webhook sent successfully"}
        except Exception as e:
            return {"status": "error", "message": str(e)}
```

## Using Action Functions

1. After creating an Action Function, it will appear in the Functions workspace
2. You can enable/disable the function using the toggle switch
3. To use the action in a chat:
   - Select the model you want to use
   - In the model settings, find the "Actions" section
   - Enable the actions you want to use with this model
   - The actions will now be available in the chat interface

## Event System

Actions can use the event system to communicate with the chat interface and other components:

### Status Updates

```python
await __event_emitter__({
    "type": "status",
    "data": {
        "description": "Processing your request...",
        "done": False
    }
})
```

### Messages

```python
await __event_emitter__({
    "type": "message",
    "data": {
        "content": "Your action has been completed successfully"
    }
})
```

### User Input

```python
response = await __event_call__({
    "type": "input",
    "data": {
        "title": "Confirmation Required",
        "message": "Do you want to proceed?",
        "placeholder": "Enter your response"
    }
})
```

### Citations

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

1. **Error Handling**: Always implement proper error handling in your action functions
2. **Input Validation**: Validate any input data before processing
3. **Security**: Be careful with sensitive data and implement appropriate security measures
4. **Performance**: Keep actions lightweight and efficient
5. **Documentation**: Provide clear documentation for your action's purpose and usage
6. **User Feedback**: Use the event system to provide clear feedback to users
7. **Resource Management**: Clean up resources after action completion

## Requirements

Action Functions can specify their requirements in the frontmatter:

```yaml
---
required_open_webui_version: "1.0.0"
requirements:
  - requests>=2.25.0
  - python-dateutil>=2.8.2
---
```

## Global Actions

Actions can be marked as global, making them available to all models by default. To make an action global:

1. Go to the Functions workspace
2. Find your action
3. Click the globe icon to toggle global status

## Troubleshooting

If your action is not working:

1. **Function Not Appearing**:
   - Check if the function is properly imported
   - Verify it's enabled in the Functions workspace
   - Ensure it's assigned to the correct model

2. **Execution Errors**:
   - Check the function logs for error messages
   - Verify all required dependencies are installed
   - Ensure the function is properly configured

3. **Event System Issues**:
   - Verify event emitter syntax
   - Check event type names
   - Ensure proper async/await usage

4. **Performance Problems**:
   - Monitor resource usage
   - Check for infinite loops
   - Verify proper cleanup

5. **Security Concerns**:
   - Review API key handling
   - Check input validation
   - Verify permission checks

## API Reference

### Endpoints

- `POST /api/chat/actions/{action_id}`: Trigger an action
  - Body: Action-specific parameters
  - Returns: Action execution result

### Response Format

```json
{
    "status": "success|error",
    "message": "Result message",
    "data": {} // Optional additional data
}
```

## Community Resources

- [Open WebUI Community](https://openwebui.com)
- [GitHub Repository](https://github.com/open-webui/open-webui)
- [Discord Community](https://discord.gg/open-webui)
- [Action Function Examples](https://openwebui.com/functions/) 