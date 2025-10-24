# MLX RAG

Explore a simple example of utilizing [MLX](https://github.com/ml-explore/mlx) for RAG application running locally on your Apple Silicon device.

I have previously converted the weights for the embedding model [gte-large](https://huggingface.co/thenlper/gte-large) into MLX format, and you can find them stored [here](https://huggingface.co/vegaluisjose/mlx-rag) in the mlx-rag repository. Additionally, as a base model, I am using [NeuralBeagle14-7B-4bit-mlx](https://huggingface.co/mlx-community/NeuralBeagle14-7B-4bit-mlx).



## Getting started

* Install requirements
```bash
python3 -m pip install -r requirements.txt
```

* Create vector database from a pdf file
```bash
python3 create_vdb.py --pdf flash_attention.pdf --vdb vdb.npz
```

* Query database (pdf file)
```bash
python3 query_vdb.py --question "what is flash attention?"
```

## Troubleshooting

### Model Loading Error: "No safetensors found"

If you encounter an error like:
```
ERROR:root:No safetensors found in /Users/[user]/.cache/huggingface/hub/models--mlx-community--NeuralBeagle14-7B-4bit-mlx/snapshots/[hash]
FileNotFoundError: No safetensors found in [path]
```

This occurs because the model weights file is named `weights.00.safetensors` but mlx_lm expects files matching the pattern `model*.safetensors`.

**Solution:**

1. First, ensure the model is fully downloaded:
```bash
python3 -c "from huggingface_hub import snapshot_download; snapshot_download('mlx-community/NeuralBeagle14-7B-4bit-mlx')"
```

2. Create a symlink with the expected naming pattern:
```bash
cd ~/.cache/huggingface/hub/models--mlx-community--NeuralBeagle14-7B-4bit-mlx/snapshots/*/
ln -s weights.00.safetensors model.safetensors
```

The model should now load correctly when running `query_vdb.py`.

