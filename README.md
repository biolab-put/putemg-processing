# putemg_examples

Dedicated exemplary scripts to be used with putEMG dataset, available at: https://biolab.put.poznan.pl/putemg-dataset/.
putemg_examples includes example scripts that can be very helpful when starting to work with putEMG dataset.
Scripts include example for filtering the raw sEMG data from the dataset, example how to use dependent putemg_features 
to calculate desired feature set. Also machine learning example, including LDA, QDA, kNN and SVM classifiers, is available.

All information about putEMG dataset is present at: https://biolab.put.poznan.pl/putemg-dataset/

## How to use putEMG examples

### 1) Download the dataset

Dataset is available at our hosting and it can be downloaded manually, but we do strongly recommend using our automated 
download script from another repository (https://github.com/biolab-put/putemg-downloader). Clone the downloader 
repository and download proper putEMG data:

```shell
> mkdir putEMG
> cd putEMG
> git clone https://github.com/biolab-put/putemg-downloader

> python3 ./putemg-downloader/putemg_downloader.py emg_gestures data-hdf5
```

### 2) Prepare repository with examples

Clone the repository with example scripts together with all dependencies, install all python dependencies:

```shell
> git clone --recursive https://github.com/biolab-put/putemg_examples.git
> python3 -m pip install numpy scipy pandas matplotlib sklearn

> cd putemg_examples
```

### 3) Run filtering example

`filter` example will apply denosing filter to a single file using bandpass filter with cutoff frequencies of 20 and 700 Hz and notch filter attenuating frequencies of 30, 50, 90, 60, and 150 Hz. Then the resulting data will be written to separate output file, eg.:

```shell
> python3 ./filter.py ../Data-HDF5/emg_gestures-14-sequential-2018-04-06-10-30-11-595.hdf5 filtered-14-sequential-1.hdf5
```

### 4) Run feature extraction example

`calculate_features` example will calculate signal features of given putEMG trial file (can be previously filtered) 
and then save it to separate output file. Features are calculated based on a feature list given in XML file 
(see https://github.com/biolab-put/putemg_features for details), eg.:

```shell
> python3 ./calculate_features.py putemg_features/all_features.xml ../Data-HDF5/emg_gestures-14-sequential-2018-04-06-10-30-11-595.hdf5 features-14-sequential-1.hdf5
```

or:

```shell
> python3 ./calculate_features.py putemg_features/all_features.xml filtered-14-sequential-1.hdf5 features-filtered-14-sequential-1.hdf5
```

### 5) Run full machine learning pipeline

`shallow_learn` is a full data processing pipeline. This will filter the data, calculate selected feature sets for all 
trials. Then data will be divided into train and test sets (with k-fold validation). Then a series of learning algorithms
will be applied (LDA, QDA, kNN, SVM) with various combinations of feature sets (RMS, Hudgins, Du). 
**Bare in mind that the process is time-consuming**. After finish results can be plotted using `shallow_learn_plot_results` 
script.

```shell
> mkdir shallow_learn_results

> python3 ./shallow_learn.py ../Data-HDF5/ ./shallow_learn_results/

> python3 ./shallow_learn_plot_results.py ./shallow_learn_results/
```

`shallow_learn` example can be used to recreate results presented in article: ***putEMG—A Surface Electromyography Hand Gesture Recognition Dataset***. When using putEMG dataset or `putemg_examples` scripts please cite: 

```text
Kaczmarek, P.; Mańkowski, T.; Tomczyński, J. putEMG—A Surface Electromyography Hand Gesture Recognition Dataset. Sensors 2019, 19, 3548. 
 
@article{putEMGKaczmarek2019,
	author = {Kaczmarek, Piotr and Mańkowski, Tomasz and Tomczyński, Jakub},
	title = {putEMG—A Surface Electromyography Hand Gesture Recognition Dataset},
	journal = {Sensors},
	volume = {19},
	year = {2019},
	number = {16},
	article-number = {3548},
	url = {https://www.mdpi.com/1424-8220/19/16/3548},
	issn = {1424-8220},
	doi = {10.3390/s19163548}
}
```

## License notes

Unless stated otherwise, all putEMG datasets elements are licensed under a Creative Commons Attribution-NonCommercial 
4.0 International (CC BY-NC 4.0). Accompanying scripts, like `putemg_examples` are licensed under a MIT License.

## Acknowledgements

This work was supported by a grant from Polish National Science Centre, project PRELUDIUM 9, research project 
no. 2015/17/N/ST6/03571.

## Dependencies
* Pandas - https://pandas.pydata.org/
* Numpy - http://www.numpy.org/
* SciPy - https://www.scipy.org/
* scikit-learn - https://scikit-learn.org/
* Matplotlib - https://matplotlib.org/

## Attributions
* PyEEG v0.4.0 - SampEn and ApEn features - GNU GPL v3 - https://github.com/forrestbao/pyeeg
