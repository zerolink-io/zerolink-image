# Zerolink Image

```bash
docker run --gpus all \
    -e HF_TOKEN=$HF_TOKEN -p 8000:8000 \
    ghcr.io/zerolinkio/zerolink-image/api:latest \
    --host 0.0.0.0 \
    --model zerolink/zsql-en-postgres
```

```bash
python -u -m vllm.entrypoints.openai.api_server \
    --host 0.0.0.0 \
    --model zerolink/zsql-en-postgres
```
