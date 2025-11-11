source:
```
https://github.com/eyaltoledano/claude-task-master/blob/main/docs/examples/claude-code-usage.md
```

https://github.com/eyaltoledano/claude-task-master/blob/main/docs/examples/claude-code-usage.md

Create a file in directory called ".taskmaster" and in there file "config.json" with following content:

```
{
  "models": {
    "main": {
      "provider": "claude-code",
      "modelId": "sonnet",
      "maxTokens": 64000,
      "temperature": 0.2
    },
    "research": {
      "provider": "claude-code",
      "modelId": "opus",
      "maxTokens": 32000,
      "temperature": 0.1
    },
    "fallback": {
      "provider": "claude-code",
      "modelId": "sonnet",
      "maxTokens": 64000,
      "temperature": 0.2
    }
  }
}
```

After that open the terminal install the taskmaster ai package: 
```
npm install -g task-master-ai
```

Run command to use sonnet or opus:
```
task-master models --set-main sonnet --claude-code
```

Create a PRD file.


