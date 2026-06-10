# CBIR Image Similarity

**Language:** [Русский](README.md) | English

An embedding-based visual search system for finding similar images.
The project allows users to upload a dataset, select or upload an extractor model, compute image embeddings, search for similar objects, and evaluate retrieval quality using retrieval metrics. Additionally, a reranking model can be uploaded for result refinement. The project makes it possible to quickly check how well different models extract image features for the task of similar object retrieval.

## Repository structure

```text
cbir-image-similarity/
├── ResNet10TI/                     # materials for the custom ResNet10TI model
├── datasets/                       # dataset examples
├── experiments/                    # experiment results
├── extractors/                     # saved extractor models
├── retrieval_reranking_CBIR.ipynb  # main notebook with the application
└── save_extractor_models.ipynb     # notebook for saving extractor models
```

## 1. Repository cloning

To use the solution, clone the repository to your device:

```bash
git clone https://github.com/MachineTrof/cbir-image-similarity.git
```

## 2. Application launch and ngrok

The project is designed to be launched through the main notebook `retrieval_reranking_CBIR.ipynb`.

For running the application through Google Colab, `ngrok` was used.

Steps:

1. Register on the `ngrok` website (https://ngrok.com).
2. Create and copy your `ngrok authtoken`.
3. Open `retrieval_reranking_CBIR.ipynb`.
4. Run all cells of the main notebook sequentially.
5. When the notebook asks for the `ngrok authtoken`, paste your token.
6. After the Streamlit application starts, open the link provided by `ngrok`.

If you do not want to use `ngrok`, it can be replaced with alternatives: `localtunnel`, `Cloudflare Tunnel`, and others.

## 3. Dataset format

The dataset must be uploaded in `.zip` format.

To calculate retrieval metrics, the dataset must follow the `ImageFolder` structure, where each folder corresponds to a separate class:

```text
dataset/
├── class_1/
│   ├── image_001.jpg
│   └── image_002.jpg
├── class_2/
│   ├── image_001.jpg
│   └── image_002.jpg
└── class_3/
    ├── image_001.jpg
    └── image_002.jpg
```

Images from the same folder are considered relevant to each other.
If classes are not found, similar image search will still work, but class-based metrics will be unavailable.

## 4. Using a custom model

The user can upload a custom model in `.pth` or `.pt` format.

The model is expected to be saved as a full PyTorch object:

```python
torch.save(model, "my_extractor.pth")
```

The model must work as an extractor, meaning that it must return an image embedding. The expected output format is `[batch_size, embedding_dim]`.

If a custom architecture is used, its classes must be added to the main notebook code between the imports and the `TEXTS` dictionary.

## 5. Adding Hugging Face models

Built-in models are defined in the `RECOMMENDED_MODEL_CONFIGS` dictionary. To add a new model from Hugging Face, specify its type and `model_id`.

If you need to add a new model type that is not supported in the current implementation, you can write an additional wrapper class by analogy with the existing `Recommended_Extractor` classes.
