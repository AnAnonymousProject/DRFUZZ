# DRFUZZ

## The Structure

- coverages: baseline coverages and the input evaluation to calculate the diversity in DRFUZZ

- dcgan: the GAN-based Fidelity Assurance Technique

- kmnc_profile: profile for baseline method DeepHunter-KMNC, which saves the boundary value of each neuron.

- models: the original models and its regression model.

- params: some params of DRFUZZ and each model/datasets

- src: the main algorithm of DRFUZZ & The experimental script


## Datasets/Models
We used 4 popular DL models based on 4 datasets on five regression scenarios.

| dataset       | model    | regression scenario                      |
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


## Usage of DRFUZZ


main.py contains all the configurations for experiment, you can use script below for experiment
~~~
python main.py --params_set alexnet fm change drfuzz --dataset FM --model Alexnet --coverage change --time 1440
~~~

params_set refers to the configuration and parameters in each dataset/model. please select according to your requirements based on files in params directory.

dataset refers to the dataset you acquired, there are totally 4 choices ["MNIST", "CIFAR10", "FM", "SVHN"]

models refers to the model you acquired, we totally designed 22 models according to 'Datasets/Models'. The setting are presented in the main.py.
please select according to Datasets/Models and your experimental settings.

coverage refers to the coverage used for experiment, please set it to 'change' if you want to use DRFUZZ, other choices are for compared approaches such as DeepHunter.

time refers to the time of experiment. we set it to 1440 minutes (24 hours) for our experiment.

Please note that, if you want to adopt the framework to your own models, please go to src/experiment_builder.py to change the path for your own models.

