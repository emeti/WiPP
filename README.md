# WiPP - A **W**orkflow for **i**mproved **P**eak **P**icking - Quick Start Guide
**WiPP** is an open source large scale GC-MS data preprocessing workflow built in Python 3 that uses machine learning to optimise, automate and combine the peak detection process of commonly used peak picking algorithms.

**WiPP** has been developed as a collaborative effort between the Berlin Institute of Health (BIH) Metabolomics platform, the BIH Core Unit Bioinformatics, the INRA Plateforme d'Exploration du Métabolisme, and the INRA Laboratoire d'Etude des Résidus et Contaminants dans les Aliments.

This document aims to help you get started with **WiPP** and brings you through the minimum requirements to install, set up, and run the software. However, we strongly recommend you to read through the [complete user guide](documentation/USERGUIDE.md) for a full and advanced use of **WiPP**.

## License
**WiPP** v 1.0 is release under the [MIT License](LICENSE.md).

## Operating System Compatibility
**WiPP** has been tested successfully with:
- Ubuntu 16 (Xenial Xerus)
- CentOS 7.6.1810 (Core)

Setup of prerequisites in Ubuntu 16 - follow the [Ubuntu-INSTALL instructions](documentation/Ubuntu-INSTALL.md)

Setup of prerequisites in CentOS 7 - follow the [CentOS-INSTALL instructions](documentation/CentOS-INSTALL.md)

Ubuntu 18 (Bionic Beaver) is not supported yet due to lacking support of incorporated R packages.

## Requirements
- conda ([Bioconda website - Python 3.x](https://conda.io/en/latest/miniconda.html))
- libnetcdf11 ([Ubuntu packages website](https://packages.ubuntu.com/xenial/libs/libnetcdf11))

> ### Note
> If you install conda from scratch, remember to `source ~/.bashrc` or open a new terminal before installing WiPP.

## Installation
You can install **WiPP** using the following command:
```bash
git clone https://github.com/bihealth/WiPP.git
cd WiPP
make
```
Now you are ready to run **WiPP**!

## Running a test project

### Change into example_project directory
The pipeline needs to be run from the project directory (the one that contains the `config.yaml` file). Use the following command to change to the example project directory:
```bash
cd ./projects/example_project
```

### Generate training data
From there, you can now run the first part of the pipeline, the generation of the training data.
You can adjust the number of cores using the inline paramter `-n <CORES>`:
```bash
../../run_WiPP.sh tr -n 4
```
> ### Note
> Running this for the first time takes a while: another conda environment is created

### Annotate detected peaks
Run the following command to start the annotation:
```bash
../../run_WiPP.sh an
```
The script opens a simple visualization tool using the default pdf viewer, and will wait for you to assign a class to the peak. By default, seven different classes are available for you to choose from.
Once you have annotated the required amount of peaks for each algorithm (only 25 for the example project), the tool will automatically close.
You are now ready to launch the last part of the pipeline.

### Do the actual peack picking
Many sequential substeps are in fact happening during this final step of the pipeline such as classifier hyperparameter optimisation, peak detection algorithms parameter optimisation, peak detection on the full dataset, output classification and result integration.
Run it with the following command and adjust the number of cores using the inline parameter `-n <CORES>`:
```bash
../../run_WiPP.sh pp -n 4
```

> ### Note
> As you annotated a very small amount of peaks, your classifier is likely not to be accurate. For this reason, we provide a trained classifier. In order to use this example classifier, please uncomment the line `path: example_classifier` (by removing the `#` symbol) in the `classifier` block of the `config.yaml` file. You can follow the same procedure for your data if you want to use a specific classifier for several projects. Please note that the example classifier is provided to help you test the tool and is specific to the test data, do not use it for your own project.


## Running your own project
To run your own project, you have to do some prelimitary work and decisions, which is described in this section.
To actually run **WiPP** subsequently, you have to follow the same steps as in the example_project.

### Input files
Let's create and structure the directory for your data files. **WiPP** supports mzML, mzData and NetCDF file formats.

You can create the `Input_folder` directory anywhere you want as long as it is accessible to the tool. You can also name it the way you want, but for clarity purposes, we will call it `Input_folder` in this tutorial.

Here is the structure:
```
Input_folder/
	condition_1/
	condition_2/
	...
	condition_n/
	Training_samples/
	Optimization_samples/
	Wash/
```

All your files should be separated into subdirectories corresponding to the experimental conditions of your study (here, condition 1 to n).

Three extra subdirectories are necessary to run the pipeline.

The `Wash` directory is optional but if present should contain the wash or blank sample files. Here, we assume that these samples contain the alkanes that are used for Retention Index calculation.

The `Training_samples` directory should contain a subset of the pooled samples or a representative subset of the samples of each biological condition. If you do not have pooled samples, we recommend using a minimum of 2 sample for each condition.

The `Optimization_samples` directory is similar to the `Training_samples` directory, just make sure to choose different samples as we do not want the parameters to be optimized using the same data the classifier was trained on.

> ### Note
> The sample files used for training and optimization should still be present in your sample directories, the files in the `Training_samples` and `Optimization_samples` directories are only copies.

### Pipeline settings

This tutorial only shows the minimum requirements to run the pipeline, to learn more about all pipeline settings, have a look at the pipeline settings section of the [complete user guide](documentation/USERGUIDE.md).

All general pipeline settings are stored in the `config.yaml` of the individual project folder and need to be created for every new project. You can have a look at the [example config](projects/example_project/config.yaml) from the example_project.

> ### Note
> By default, in the example project, we run only one peak picking algorithm to speed up the test. Do not forget to enable in the `config.yaml` file the peak picking algorithms you want to use.

First, you need to define the absolute path to your `Input_folder`. This parameter can be found in the `static_data` block under the name `absolute_path`. You also need to specify the resolution of your data by setting the `high_resolution` parameter to `True` or `False` (in this context, any data with a mass resolution higher than 1 Da is considered high resolution).

Next, if you have a `Wash` directory for retention index calculation, go to the `retention_index` block. You need to define the relative path from the `Input_folder` to the `Wash` folder. In our example, this parameter would look like this `./Wash`.
You also have to define the alkanes present in your samples as a simple list `c10,c12,c16,[...],c32,c34`.
If you do not have blank samples containing alkanes for retention index calculation, you can leave this section as it is in the example file.

The last two compulsory parameters that you need to define are the relatives path from the `Input_folder` to your `Training_samples` and `Optimization_samples` directories, respectively found in `training_data-general` and `optimization-general` setting blocks.

That's all for the basic settings, you are now ready to run the pipeline.

> ### Note
> Keep in mind that those parameters are the only one required to run the pipeline, but there is a lot more you can do to precisely tune the pipeline. Have a read through the [complete user guide](documentation/USERGUIDE.md) to learn more.

### Running the pipeline
Follow the same steps as in the [example_project](#running-a-test-project).

> ### Note
> Peak annotation usually takes several hours (1200 peaks per algorithm by default), but you only need to do that once per instrument/protocol. Once trained, the SVM classifiers can be reused on other datasets generated with the same instrument and data acquisition method.
