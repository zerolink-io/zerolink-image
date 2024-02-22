# Zerolink Image

This is repository for the official Zerolink deployment image. This image runs
on a cloud provider which supports GPU acceleration and is used to serve
customer built text-to-sql model via an OpenAI-compatible API server.

Download the latest image from the GitHub Container Registry:

```bash
docker pull ghcr.io/zerolink-io/zerolink-image:main
```

To run the API server in a docker container, you can use the following command:

```bash
docker run --gpus all \
    -e HF_TOKEN=$HF_TOKEN \
    -p 8000:8000 \
    ghcr.io/zerolink-io/zerolink-image:main \
    --host 0.0.0.0 \
    --model zerolink/zsql-en-postgres
```

To run the API server locally, you can use the following command:

```bash
python -u -m vllm.entrypoints.openai.api_server \
    --host 0.0.0.0 \
    --model zerolink/zsql-en-postgres
```

## License

This repository is licensed under the Apache 2.0 License. See the
[LICENSE](LICENSE) file for more information.
