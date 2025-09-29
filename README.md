# 🦾 TextCRS

This repository provides the official implementation and extension for **Text-CRS: A Generalized Certified Robustness Framework against Textual Adversarial Attacks**. The project was completed as part of the Clemson CPSC 8570 NTS course.

**Group members:**  
- Danish Bhatkar  
- Gaurav Patel  
- Sarthak Nikhal  
- Mithilesh Biradar  

## ⚙️ Installation

Our code is implemented and evaluated on **Python 3.9** and **PyTorch 1.11**.

Install all dependencies:  
```pip install -r requirements.txt```

## 🚀 Usage

### 🗃️ Prepare Datasets

Text classification datasets are pre-downloaded to `./datasets`: **AG’s News** and **IMDB**.  
The `data/xinyu/results` directory is empty and must be populated separately.  
Pickled datasets can be downloaded [here](https://drive.google.com/file/d/1YkIDHRc2VwQizK6YTwrTWu9w6Jk3KBWV/view?usp=sharing).

### 🔁 Repeat Experiments

#### 🏋️‍♂️ Train

Select your training parameters:

- Noise type (e.g., ```-if_addnoise 5, 8, 7, 4```)
- Model (```-model_type lstm```, ```bert```, or ```cnn```)
- Dataset (```-dataset agnews, amazon, or imdb```)

To train the smoothed classifier, run commands such as:

**Certified Robustness to Synonym Substitution:**  ```-syn_size 50, 100, 250``` (i.e., $s$ in Table 4).

```
python textatk_train.py -mode train -dataset amazon -model_type lstm -if_addnoise 5 -syn_size 50
```

**Certified Robustness to Word Reordering:**  ```-shuffle_len 64, 128, 256``` (i.e., $2\lambda$ in Table 4).

```
python textatk_train.py -mode train -dataset amazon -model_type lstm -if_addnoise 8 -shuffle_len 256
```

**Certified Robustness to Word Insertion:**  ```-noise_sd 0.5, 1.0, 1.5``` (i.e., $\sigma$ in Table 4).

```
python textatk_train.py -mode train -dataset amazon -model_type newbert -if_addnoise 7 -noise_sd 0.5
```

**Certified Robustness to Word Deletion:**  ```-beta 0.3, 0.5, 0.7``` (i.e., $p$ in Table 4).

```
python textatk_train.py -mode train -dataset amazon -model_type lstm -if_addnoise 4 -beta 0.3
```

#### 🛡️ Certify

Choose your noise type, model, and dataset.  
Then run the corresponding shell script, e.g.: 

```
sh ./run_shell/certify/certify/noise4/lstm_agnews_certify.sh
```

## ⚔️ Adversarial Attacks

#### ✏️ Generate Adversarial Examples

Adversarial attack code in `./textattacknew` is extended from the [TextAttack](https://github.com/QData/TextAttack/) project.

Specify attack parameters:

- Model (```-model_type lstm```, ```bert```, or ```cnn```)
- Dataset (```-dataset agnews```, ```amazon```, or ```imdb```)
- Attack type (```-atk textfooler```, ```swap```, ```insert```, ```bae_i```, ```delete```)
- Number of adversarial examples (e.g., ```-num_examples 500```)

Example command:

```
python textatk_attack.py -model_type cnn -dataset amazon -atk textfooler -num_examples 500 -mode test
```

#### 🛡️ Certify (with adversarial data) 

Use the above ```.sh``` shell script, adding ```-ae_data $AE_DATA```:

```
sh ./run_shell/certify/certify/noise4/lstm_agnews_certify.sh
```


## Citation

```
@inproceedings{zhang2023text,
title={Text-CRS: A Generalized Certified Robustness Framework against Textual Adversarial Attacks},
author={Zhang, Xinyu and Hong, Hanbin and Hong, Yuan and Huang, Peng and Wang, Binghui and Ba, Zhongjie and Ren, Kui},
booktitle={2024 IEEE Symposium on Security and Privacy (SP)},
pages={53--53},
year={2023},
organization={IEEE Computer Society}
}
```


## 🙏 Acknowledgements

- [TextAttack](https://github.com/QData/TextAttack)
- [Original Repo](https://github.com/Eyr3/TextCRS)


