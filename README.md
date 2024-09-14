# Docker Nginx OpenAI API Cache Reverse proxy

This project is a simple Docker Nginx project that serves as a cache for the OpenAI API.

nginx here is preconfigured to work on OpenAI API.

### Features:

- **Works with any client** that allows you to configure the server address (as it acts as a reverse proxy)
- Caches the response of the support endpoints. The key of cache is built from the request uri and body
- Caches the response for the POST requests only
- Use multiple LLM providers
- Returns an "X-Cache-Status" header indicating whether the response was served from cache or not

### Supported endpoints:

- POST /v1/* (default)
- POST /openai/* (openai)
- POST /openrouter/* (openrouter)
- POST /anthropic/* (anthropic)
- POST /groq/* (groq)
- POST /mistral/* (mistral)
- POST /cohere/* (cohere)
- POST /ai21/* (ai21)

## Getting Started

### Prerequisites

- Docker
- Docker compose

### Installation

1. Clone the repository:

```
git clone https://github.com/gpt4thewin/docker-nginx-openai-api-cache.git
cd docker-nginx-openai-api-cache
```

2. Start the container:

```
docker-compose up -d
```

3. Test the server

Setup your credentials:
```
OPENAI_API_KEY="...."
```

Run this 2 times or more:
```
curl -s -o /dev/null -w "%{http_code}" http://localhost:81/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Hello there !"
    }
  ],
  "temperature": 0,
  "max_tokens": 228,
  "top_p": 1,
  "frequency_penalty": 0,
  "presence_penalty": 0
}'
```

4. Check the logs

```
docker-compose logs
```

The last lines should show something like this
```
openai-cache-proxy  | 172.28.0.1 - - [29/Feb/2024:19:59:49 +0000] "POST /v1/chat/completions HTTP/1.1" 200 494 "-" "curl/7.80.0" Cache: MISS
openai-cache-proxy  | 172.28.0.1 - - [29/Feb/2024:19:59:52 +0000] "POST /v1/chat/completions HTTP/1.1" 200 494 "-" "curl/7.80.0" Cache: HIT
```

5. Stop the container:

```
docker-compose down
```

### Usage

Set your client's API server address to `http://localhost:81/v1`
Once the containers are running, you can use the OpenAI API through the cache by sending requests to the supported URIs.

URIs that are supported will be forwarded, unless they are cached. 
URIs that are not supported will be forwarded normally.

### Configuration

The cache is configured using the `nginx.conf`. You can modify this file to change the cache settings or add additional URIs.

## Contributing

Contributions are welcome! Please submit a pull request or open an issue if you encounter any problems or have suggestions for improvements.
