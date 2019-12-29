## Introduction
This notebook (albert_toy) illustrates how to 
1. pretrain Albert with custom corpus
2. further fine tune the pretrained Albert 
This notebook illustrates with example use case as, with a corpus of restaurant reviews to extract if there are any dish names in the restaurant review. The whole corpus of restaurant reviews will be used as custom corpus to pretrain Albert model. The the pretrained albert model would be further fine tuned on custom dish name recognition task (like a custom NER), with training data as labelled restaurant review indicating dish name tokens. 

## Dataset
Restaurant reviews: 
["I like the mala steamboat a lot!",
"The chicken rice doesn't taste nice.] 

Dish name from the reviews respectively, to be used to create training data as labelled restaurant review indicating dish name tokens
["mala steamboat",
"chicken rice"]

## Steps
### 1. Build Vocab for own custom corpus
#### Overview
Google didnt open source the wordpeice unsupervised tokenizer, we can modify on open sourced SentencePiece or t2t text encoder subword builder to generate vocab that is compatible with bert/albert models.

An open source implementation of modifying t2t text encoder subword builder could be found at https://github.com/kwonmha/bert-vocab-builder
#### Steps
1. Clone the repo (Create environment and install requirements accordingly)
2. Prepare corpus files 
3. run the command in the repo directly python subword_builder.py --corpus_filepattern "{corpus_for_vocab}" --output_filename {name_of_vocab} --min_count {minimum_subtoken_counts}

### 2. Pretrain Albert with own custom corpus
#### Overview
Repo: https://github.com/google-research/ALBERT
#### Steps
1. Clone the repo (Create environment and install requirements accordingly)
2. Create Albert pretrain files: run the command in the repo directly using corpus and vocab used in Build Vocab step, python create_pretraining_data.py --input_file "{corpus}" --output_file {output_file} --vocab_file {vocab_file}
3. Pretrain albert model
    1. Prepare albert_config_file: note vocab_size in albert_config_file should be same as the total vocab in Build Vocab step. Refer to https://tfhub.dev/google/albert_base/2 for a ablert_base_v2 config file
    2. Run the command in the repo directly, python run_pretraining.py --input_file={albert_pretrain_file} --output_dir={models_dir} --albert_config_file={albert_config_file}

### 3. Finetune Albert with downstream tasks
#### Overview
Repo: https://github.com/google-research/ALBERT
Reference to run_classifier.py in ALBERT repo. Details refer to the notebook in this repo.
