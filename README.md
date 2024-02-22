# Zerolink Image

This is repository for the official Zerolink deployment image. This image runs
on a cloud provider which supports GPU acceleration and is used to serve
customer built text-to-sql model via an OpenAI-compatible API server.

Download the latest image from the GitHub Container Registry:

```bash
docker pull ghcr.io/zerolink-io/zerolink-image:main
```

To run the API server in a docker container, you can use the following command.
Change the model name (i.e. `zerolink/zsql-en-postgres`) to the model you want
to use and optionally set the `HF_TOKEN` environment variable to your Hugging
Face API in order to use private models.

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

## Usage

Since this server is compatible with OpenAI API, you can use it as a drop-in
replacement for any applications using OpenAI API. For example, you can
interact with a local model server using the following Python code:

```python
from openai import OpenAI
openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

prompt = f"""
Using the schema:
{schema}
Generate SQL for the following question:
{question}
"""

chat_response = client.chat.completions.create(
    model="zerolink/zsql-en-postgres",
    messages=[
        {"role": "system", "content": "Translate English to Postgres SQL."},
        {"role": "user", "content": prompt},
    ]
)
print("Response:", chat_response)
```

## License

This repository is licensed under the Apache 2.0 License. See the
[LICENSE](LICENSE) file for more information.
