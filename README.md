**Pre**

If you got the time, hardware and basic linux knowledge you can setup ollama with Open Web UI and perform queries from EspoCRM.

This is not a step-by-step tutorial, this is only a proof of concept. This is a higher investment cost and lower subscription cost method. If you don't have the knowledge you may want to use a higher subscription cost and lower investment cost method since this
also require you to maintain your own AI server.

If you do want help or pointers, we can provide paid support. Email us at kontakt@novastream.se.

**Requirements**

- EspoCRM with Advanced Pack
- Server with a mid-range CPU / 8-16+ GB RAM / 1+ high range nvidia GPU or 2+ mid range GPUs

**Setup ollama, Open Web UI and llama3:latest model**

Please have a look at this video: https://www.youtube.com/watch?v=Wjrdr0NU4Sk

I would recommend:

- Linux instead of Windows with WSL
- nginx proxy infront of Open Web Ui docker with a ssl certificate and best practice nginx configs (https://github.com/h5bp/server-configs-nginx)
- llama3 model instead of llama2

**EspoCRM modifications**
- Modify the case entity and add a text field called eg: aiResponse
- Add the aiResponse field to your detail layout
- Add a workflow with a Manual trigger for the Case entity (name the button Ask AI or something similar)

**Use the following HTTP Send Request Action**

Type of request: POST

URL: https://myaiserver.com/ollama/api/generate

Headers: Authorization: Bearer sk-123456789012345678901234567890

Headers: Content-Type: application/json

Payload: { "model": "llama3:latest", "prompt": "Could you find a solution (keep it short) on how to solve the following issue. Please respond in the question language. Question: {$description}", "stream": false }

**Use the following followup action**

Type: Update target entity

Formula: cAiResponse = json\retrieve(workflow\lastHttpResponseBody(), 'response');

**Notes**
- You may want to adjust workflowSendRequestTimeout in config.php to something like 30-90 seconds depending on hardware
