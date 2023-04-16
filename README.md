# Docker Nginx OpenAI API Cache Reverse proxy

This project is a simple Docker Nginx project that serves as a cache for the OpenAI API. It will cache response of the following endpoints:

- POST /v1/chat/completions
- POST /v1/completions
- POST /v1/edits
- POST /v1/embeddings
- POST /v1/moderations
- POST /v1/answers

Will also cache the following endpoints (deprecated by OpenAI):

- POST /v1/engines/*/chat/completions
- POST /v1/engines/*/completions
- POST /v1/engines/*/edits
- POST /v1/engines/*/embeddings
- POST /v1/engines/*/moderations
- POST /v1/engines/*/answers

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

3. Start the container:

```
docker-compose up -d
```

4. Follow the logs

```
docker-compose logs -f
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
