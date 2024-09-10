# Deploying with Flex AI

## Prerequisites

- Hugging Face account and token
- Flex AI CLI installed
- Access to FCS (Flex Compute Service)

## Configuration

1. In the [config file](./training/config/preprocess.json), specify:
   - Your Hugging Face token (`token`)
   - The path to save the dataset (`preprocessed_dataset_hub_path`)

## Steps

### 1. Add Source

Add your model source to Flex AI:

```bash
flexai source add <model_source_name> <github_url>
```

Replace `<model_source_name>` with your actual source name.

### 2. Choose Dataset

The dataset is loaded directly from the Hugging Face Hub during the training phase. Choose a small dataset from FCS to reduce build time. The Hugging Face dataset name must be specified in the [config JSON file](./training/config/preprocess.json).

### 3. Launch Training

#### Incorrect Way (Will Fail)

```bash
flexai training run <name_of_the_training> \
    --source-name <model_source_name> \
    --source-revision <revision> \
    --dataset <your_dataset_name> \
    -- ./training/run_parler_tts_training.py ./training/config/preprocess.json
```

This method is expected to fail without an error message (it will get stuck).

#### Correct Way

```bash
flexai training run <name_of_the_training> \
    --source-name <model_source_name> \
    --source-revision <revision> \
    --dataset <your_dataset_name> \
    -- -m accelerate.commands.launch ./training/run_parler_tts_training.py ./training/config/preprocess.json
```

This method should succeed.

## Notes

- Ensure all placeholders (e.g., `<name_of_the_training>`, `<model_source_name>`, `<your_dataset_name>`) are replaced with your actual values.
- The key difference in the correct method is the use of `accelerate.commands.launch`, which properly initializes the training process.
