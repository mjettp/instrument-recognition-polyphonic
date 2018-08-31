# instrument-recognition-polyphonic
## Requirements: 
* MedleyDB (can be downloaded from here : http://medleydb.weebly.com/downloads.html)

For the deep learning part:
* GPU 
* keras 1.2.2
* librosa
* numpy
* pandas
* sklearn

Organization of the respository:  

The source code is organized as follows -  

The ./settings.py contains all the paths to different folders relevant to the experiments that need to be set. The data_prep folder contains the code required to preprocess MedleyDB dataset. The models folder contains folders for deep-learning and traditional machine learning code.

Sequence of execution along with brief decription for each of the files:  

## Data Preprocessing  
1) ./data_prep/data_prep.py 
This file contains all scripts necessary for preparing data.
Usage :
```bash
python data_prep.py -c window_configuration
```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted.  
For example,
```bash
python data_prep.py -c _5s_h50

```

2) ./data_prep/gen_split.py  
This file splits the group of .mat files generated by running data_prep.py into 5 sets, each containing 20% of the samples.
Usage :
```bash
python gen_split.py -c window_configuration
```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted.  
For example,
```bash
python gen_split.py -c _5s_h50

```

3) ./data_prep/wav_generator.py  
The wavfiles for each of the samples which were split using gen_split.py are generated using this code.
Usage :
```bash
python wav_generator.py -c window_configuration
```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted.  
For example,
```bash
python wav_generator.py -c _5s_h50

```

4)./data_prep/audio_transformer.py  
This code splits an audio file into harmonic component and residual component using librosa and stores them separately.
Usage :
```bash
python audio_transformer.py -c window_configuration
```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted.  
For example,
```bash
python audio_transformer.py -c _5s_h50

```

## for traditonal method:  
1)./models/traditional/feature_extractor.py  
This code makes use of Essentia's music extractor to extract temporal, spectral and cepstral features from the wav files and persist them.
Usage :
```bash
python feature_extractor.py -c window_configuration -t dataset_type

```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted. The dataset_types are : {original, harmonic and residual}.
For example,
```bash
python feature_extractor.py -c _5s_h50 -t original

```

2)./models/traditional/train_accumulator.py  
This code aggregates datasets {0,1,2,3} and their respective labels into training set.
Usage :
```bash
python train_accumulator.py -c window_configuration -t dataset_type

```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted. The dataset_types are : {original, harmonic and residual}.
For example,
```bash
python train_accumulator.py -c _5s_h50 -t original

```

3)./models/traditional/test_accumulator.py  
This code aggregates dataset_4 and their respective labels into test set.
Usage :
```bash
python test_accumulator.py -c window_configuration -t dataset_type

```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted. The dataset_types are : {original, harmonic and residual}.
For example,
```bash
python test_accumulator.py -c _5s_h50 -t original

```

4)./models/traditional/regressor.py  
This code fits a regressor on training dataset and then predicts the instrument annotations for the test set.
Usage :
```bash
python regressor.py -c window_configuration -t dataset_type

```
The window_configurations are : _{window_size}_h{percentage_hop} for which datasets have already been extracted. The dataset_types are : {original, harmonic and residual}.
For example,
```bash
python regressor.py -c _5s_h50 -t original

```

## for deep-learning:  
Refer to ./models/deep-learning/README.me

