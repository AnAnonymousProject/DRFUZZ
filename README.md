# DRFUZZ

## Description

`DRFuzz` is a novel regression fuzzing technique for DL systems by generating fault-triggering inputs with high fidelity and diversity. `DRFuzz` adopts the MCMC strategy to select mutation rules that are prone to generating fault-triggering test inputs and proposes a GAN-based fidelity assurance method to ensure the fidelity of the generated inputs. Also, `DRFuzz` incorporates tree-based seed pool trimming and seed probability decay to maintain the quality of seed
pool, and thus increases the diversity of the detected regression faults. We conducted an empirical study to evaluate the effectiveness of `DRFuzz` on four subjects in five regression scenarios, and the results show that `DRFuzz` outperforms state-of-the-art techniques in detecting unique regression faults across all the subjects and scenarios. Moreover, `DRFuzz` can help improve the defense capability of the models against existing black-box attacking techniques

## The Structure

- coverages: baseline coverages and the input evaluation to calculate the diversity in `DRFuzz`

- dcgan: the GAN-based Fidelity Assurance Technique

- kmnc_profile: profile for baseline method DeepHunter-KMNC, which saves the boundary value of each neuron.

- models: the original models and its regression model.

- params: some params of `DRFuzz` and each model/datasets

- src: the main algorithm of DRFUZZ & The experimental script


## Datasets/Models
We used 4 popular DL models based on 4 datasets on five regression scenarios, as the initial seed models in `DRFuzz`, which have been widely used in many existing studies.

| Dataset       | Model    | Regression Scenario                      |
| ------------- | -------- | ---------------------------------------- |
| MNIST         | LeNet5   | SUPPLY,ADV:BIM,ADV:CW,FIXING,PRUNE,QUANT |
| CIFAR10       | VGG16    | SUPPLY,ADV:BIM,ADV:CW,FIXING,PRUNE       |
| Fashion-MNIST | Alexnet  | SUPPLY,ADV:BIM,ADV:CW,FIXING,PRUNE,QUANT |
| SVHN          | ResNet18 | SUPPLY,ADV:BIM,ADV:CW,FIXING,PRUNE       |
 


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
