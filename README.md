# KEPT

This repository contains the implementation of our KEPT model on the auto icd coding task presented in xxx.

## Dependencies

* Python 3
* [NumPy](http://www.numpy.org/)
* [PyTorch](http://pytorch.org/) (currently tested on version 1.9.0+cu111)
* [Transformers](https://github.com/huggingface/transformers) (currently tested on version 4.16.2)

Full environment setting is lised [here](conda-environment.yaml).

## Download / preprocess data
One need to obtain licences to download MIMIC-III dataset. Once you obtain the MIMIC-III dataset, please follow [caml-mimic](https://github.com/jamesmullenbach/caml-mimic) to preprocess the dataset. You should obtain train_full.csv, test_full.csv, dev_full.csv, train_50.csv, test_50.csv, dev_50.csv after preprocessing.
Quickroutes are provided to save time, you could download processed data 
(
[GDrive](https://drive.google.com/file/d/1QCc2BACIgv4d5Q5jDMxM5iSC4hOIv-fU/view?usp=sharing)
) 
and unzip in this folder.

TODO: change to script

## Modify constant
Modify constant.py : change DATA_DIR to where your preprocessed data located.

Modify wandb logging by changing run_coder.py this line wandb.init(project="mimic_coder", entity="whaleloops")


## Train and Eval

MIMIC-III 50 (2 A100 GPU):

```
CUDA_VISIBLE_DEVICES=0,1 python -m torch.distributed.launch --nproc_per_node 2 --master_port 57666 run_coder.py \
                --ddp_find_unused_parameters False \
                --disable_tqdm True \
                --version mimic3-50 --model_name_or_path MODEL_NAME_OR_PATH \
                --do_train --do_eval --do_predict --max_seq_length 8192 \
                --per_device_train_batch_size 1 --per_device_eval_batch_size 2 \
                --learning_rate 1.5e-5 --weight_decay 1e-3 --adam_epsilon 1e-7 --num_train_epochs 8 \
                --evaluation_strategy epoch --save_strategy epoch \
                --logging_first_step \
                --output_dir ./saved_models/longformer-original-clinical-prompt2alpha
```

## Citation

Please cite the following if you find this repo useful.


## License

See the [LICENSE](LICENSE) file for more details.

