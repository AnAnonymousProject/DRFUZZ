# DRFUZZ

## Description

`DRFuzz` is a novel regression fuzzing technique for DL systems by generating fault-triggering inputs with high fidelity and diversity. `DRFuzz` adopts the MCMC strategy to select mutation rules that are prone to generating fault-triggering test inputs and proposes a GAN-based fidelity assurance method to ensure the fidelity of the generated inputs. Also, `DRFuzz` incorporates tree-based seed pool trimming and seed probability decay to maintain the quality of seed
pool, and thus increases the diversity of the detected regression faults. We conducted an empirical study to evaluate the effectiveness of `DRFuzz` on four subjects in five regression scenarios, and the results show that `DRFuzz` outperforms state-of-the-art techniques in detecting unique regression faults across all the subjects and scenarios. Moreover, `DRFuzz` can help improve the defense capability of the models against existing black-box attacking techniques

## The Structure

Here, we briefly introduce the usage/function of each directory: 

- `coverages`: baseline coverages and the input evaluation to calculate the diversity in `DRFuzz`
- `dcgan`: the GAN-based Fidelity Assurance Technique (the DCGAN structure and prediction code)
- `kmnc_profile`: profile for compared approach DeepHunter-KMNC, which saves the boundary value of each neuron.（Please note that, This is only for DeepHunter-KMNC following the implementation of its source code to improve the efficiency of KMNC）
- `dataset`: the stored dataset
- `models`: the original models and its regression model. (Since  the file size of some models are large, here we provide all the models and regression model on MNIST-LeNet5 for the reproduction)
- `params`: some params of `DRFuzz` and each model/datasets
- `src`: the main algorithm of DRFUZZ & The experimental script

## Datasets/Models

We use 4 popular DL models based on 4 datasets under five regression scenarios, as the initial seed models in `DRFuzz`, which have been widely used in many existing studies.

| ID   | model    | dataset | M1_acc | M2_acc | Scenario |
| ---- | -------- | ------- | ------ | ------ | -------- |
| 1    | LeNet5   | MNIST   | 85.87% | 97.83% | SUPPLY   |
| 2    | LeNet5   | MNIST   | 98.07% | 97.5%  | ADV:BIM  |
| 3    | LeNet5   | MNIST   | 98.07% | 98.3%  | ADV:CW   |
| 4    | LeNet5   | MNIST   | 98.07% | 98.12% | FIXING   |
| 5    | LeNet5   | MNIST   | 98.07% | 98.12% | PRUNE    |
| 6    | LeNet5   | MNIST   | 98.07% | 98.06% | QUANT    |
| 7    | VGG16    | CIFAR10 | 87.67% | 87.88% | SUPPLY   |
| 8    | VGG16    | CIFAR10 | 87.92% | 87.51% | ADV:BIM  |
| 9    | VGG16    | CIFAR10 | 87.92% | 88%    | ADV:CW   |
| 10   | VGG16    | CIFAR10 | 87.92% | 88.4%  | FIXING   |
| 11   | VGG16    | CIFAR10 | 87.92% | 76.27% | PRUNE    |
| 12   | AlexNet  | FM      | 89.33% | 90.34% | SUPPLY   |
| 13   | AlexNet  | FM      | 91.7%  | 90.96% | ADV:BIM  |
| 14   | AlexNet  | FM      | 91.7%  | 91.87% | ADV:CW   |
| 15   | AlexNet  | FM      | 91.7%  | 92.9%  | FIXING   |
| 16   | AlexNet  | FM      | 91.7%  | 91.54% | PRUNE    |
| 17   | AlexNet  | FM      | 91.7%  | 91.78% | QUANT    |
| 18   | ResNet18 | SVHN    | 88.85% | 91.93% | SUPPLY   |
| 19   | ResNet18 | SVHN    | 92.05% | 91.9%  | ADV:BIM  |
| 20   | ResNet18 | SVHN    | 92.05% | 92.01% | ADV:CW   |
| 21   | ResNet18 | SVHN    | 92.05% | 92.1%  | FIXING   |
| 22   | ResNet18 | SVHN    | 92.05% | 91%    | PRUNE    |


1: We design 5 regression scenarios: supplementary training (denoted as SUPPLY), adversarial  training (denoted as ADV), white-box model fixing (denoted as FIXING), Model Pruning(denoted as PRUNE), Model quantization (denoted as QUANT). 

2: For SUPPY, we select 80% of training set to train original model as use 20% remaining data for supplementary training.

3: For ADV, we provided adversarial  training on 2 kinds of adversarial examples. (C&W and BIM)
 
4: The overall result is saved here named `overall_result.jpg` due to the limited paper space.

## The Requirements:

- python==3.7

- keras==2.3.1 

- tensorflow==1.15.0 

- cleverhans==3.0.1  

- h5py==2.10.0


## Reproducibility

### Environment

We conducted 22 experiments in `DRFuzz` of which the libraries are as described above. 

**Step 0:** Please install the above runtime environment.

**Step 1:** Clone this repository. Download the dataset and models from OneDrive. 
Save the code and unzip datasets and models to `/your/local/path/`, e.g. `/your/local/path/models` and `/your/local/path/dataset`. 
(`/your/local/path/` should be the absolute path on your server, e.g. `/home/user_xxx/`) 

**Step 2:** Train yourself DCGAN models and save them to `/your/local/path/dcgan`

**Step 3:** Edit configuration files `/your/local/path/src/experiment_builder.py` and `/your/local/path/dcgan/DCGAN_utils.py` in order to set the dataset, model and DCGAN model path into `DRFuzz`

### Running DRFuzz

The `DRFuzz` artifacts are well organized, and researchers can simply run `DRFuzz` with the following command.

~~~
python main.py --params_set alexnet fm change drfuzz --dataset FM --model Alexnet --coverage change --time 1440
~~~

main.py contains all the configurations for experiment.

`--params_set` refers to the configuration and parameters in each dataset/model. please select according to your requirements based on files in params directory. 
            If you want to adopt your own patams, please go to `your/local/path/params` to change the setting params.

`--dataset` refers to the dataset you acquired, there are totally 4 choices ["MNIST", "CIFAR10", "FM", "SVHN"]

`--models` refers to the model you acquired, we totally designed 22 models according to 'Datasets/Models'. The setting are presented in the main.py. 
            Please select according to Datasets/Models and your experimental settings.

`--coverage` refers to the coverage used for experiment, please set it to 'change' if you want to use `DRFuzz`, other choices are for compared approaches such as DeepHunter.

`--time` refers to the time of experiment. we set it to 1440 minutes (24 hours) for our experiment.

### RQ2 and its accuracy against test set
|                       | train on   | on DiffChaser | on DeepHunter | on DRFuzz | testset |
| --------------------- | ---------- | ------------- | ------------- | --------- | ------- |
| mnist_lenet5_sup      | diffchaser | -             | 41.13%        | 64.06%    | 99.14%  |
|                       | deephunter | 85.62%        | -             | 73.88%    | 99.09%  |
|                       | DRFuzz     | 96.70%        | 63.80%        | -         | 99.26%  |
| mnist_lenet5_adv_cw   | diffchaser | -             | 34.01%        | 54.78%    | 98.15%  |
|                       | deephunter | 83.99%        | -             | 66.74%    | 98.15%  |
|                       | DRFuzz     | 76.08%        | 61.83%        | -         | 98.49%  |
| mnist_lenet5_adv_bim  | diffchaser | -             | 27.01%        | 49.10%    | 98.34%  |
|                       | deephunter | 74.20%        | -             | 57.66%    | 98.43%  |
|                       | DRFuzz     | 78.72%        | 42.46%        | -         | 98.59%  |
| cifar10_vgg16_tr      | diffchaser | -             | 62.77%        | 63.46%    | 88.19%  |
|                       | deephunter | 64.92%        | -             | 69.10%    | 86.64%  |
|                       | DRFuzz     | 91.40%        | 81.28%        | -         | 88.41%  |
| cifar10_vgg16_adv_cw  | diffchaser | -             | 46.15%        | 53.96%    | 86.15%  |
|                       | deephunter | 51.08%        | -             | 55.26%    | 86.32%  |
|                       | DRFuzz     | 72.23%        | 75.30%        | -         | 87.34%  |
| cifar10_vgg16_adv_bim | diffchaser | -             | 55.59%        | 56.88%    | 85.49%  |
|                       | deephunter | 70.26%        | -             | 70.20%    | 88.21%  |
|                       | DRFuzz     | 91.92%        | 79.41%        | -         | 89.03%  |
| fm_alexnet_tr         | diffchaser | -             | 48.20%        | 46.56%    | 91.04%  |
|                       | deephunter | 63.02%        | -             | 69.54%    | 91.02%  |
|                       | DRFuzz     | 91.50%        | 70.80%        | -         | 91.65%  |
| fm_alexnet_adv_cw     | diffchaser | -             | 52.57%        | 46.04%    | 90.29%  |
|                       | deephunter | 55.04%        | -             | 59.42%    | 91.32%  |
|                       | DRFuzz     | 77.84%        | 65.84%        | -         | 92.01%  |
| fm_alexnet_adv_bim    | diffchaser | -             | 52.14%        | 49.58%    | 91.61%  |
|                       | deephunter | 62.38%        | -             | 66.66%    | 91.83%  |
|                       | DRFuzz     | 89.92%        | 73.78%        | -         | 91.95%  |
| svhn_resnet18_tr      | diffchaser | -             | 65.16%        | 59.34%    | 90.03%  |
|                       | deephunter | 82.54%        | -             | 67.44%    | 92.64%  |
|                       | DRFuzz     | 88.11%        | 81.46%        | -         | 91.40%  |
| svhn_resnet18_adv_cw  | diffchaser | -             | 66.67%        | 62.12%    | 90.37%  |
|                       | deephunter | 50.00%        | -             | 56.64%    | 89.72%  |
|                       | DRFuzz     | 92.16%        | 81.06%        | -         | 93.22%  |
| svhn_resnet18_adv_bim | diffchaser | -             | 73.53%        | 63.58%    | 91.77%  |
|                       | deephunter | 78.03%        | -             | 70.92%    | 92.22%  |
|                       | DRFuzz     | 98.44%        | 87.40%        | -         | 94.85%  |
