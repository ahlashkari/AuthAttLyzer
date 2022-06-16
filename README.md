# AuthAttLyzer (Authorship Attribution Analyzer)





### Python based source code Authorship Attribution 

This directory contains all the code to perform authorship attribution based on the extracted features 

### About

In particular, you'll find here the code for the following two groups of experiments:

*Code Authorship Attribution.* Extract features for a dataset and try to assign source code to authors.

### Setup
In this directory, you will find a *requirements.txt* and *config_example.ini*
file. We will need both files in the following.

1. Create a virtualenv or conda environment (strongly recommended).
I've tested everything with conda. Use Python 3.6, with conda,
you can use ```conda create --name authorshipevasion python=3.6```.
Then activate the environment via ```conda activate authorshipevasion```.

2. Install via ```pip install -r requirements.txt``` the dependencies in your python environment. I strongly recommend using a virtualenv or
conda environment.

3. Adjust the configuration in AuthAttLyzer

  - The config file saves all paths that we need to consider, such as path to this repo, path to clang, etc.
  What you have to do:
      1. Copy config_example.ini to config.ini
      2. Adjust config.ini to your own paths
      3. For IWYU, use a path like: `/home/<YOUR_PATH>/iwyu/include-what-you-use/build/`
  - Assure that Configuration.py and config.ini are in the same directory.

4. Unzip the dataset_2017 file in the data folder


### Attribution
This section describes how to perform authorship attribution.
You can use the code as a stand-alone code for attribution tasks or you will
need this step as a requirement for the attacks, of course.

You can either use our dataset (Google Code Jam contest 2017) or
your own dataset. If you're using our dataset, you do not need to
consider the *dataset* step, as the formatted dataset
is also part of the repository.

(Do not forget to look at our tutorial in the evaluations directory!)

## Dataset & Features

### Dataset [Optional]
- Under the repository's root directory in data/dataset_2017, you will find the
used dataset.
- We added the raw dataset, the formatted dataset via clang-format, and
the dataset where we also processed macros.

- If you have a new dataset, make sure you have the following format:
  - In your dataset directory, each author should have its own directory.
  - Each author directory should contain all the challenges/problems.
  - Each file should have the following format: ```<round-id>_<challenge-id>_<authorname>.cpp```

- Then, run the following scripts in this order:
  - clangformat.sh
  - remove_macros.sh
  - Adjust the paths of course.

### Features
First of all, we need to extract the features for a dataset.
We've build the extractors with clang, so you need to compile at least
the feature extraction commands.

1. Now go to the data directory under the project's root directory.
2. You will find there a extractfeatures_single.sh.
  - Adjust the relative paths to the dataset.
    - Adjust the variables SRC and OUTPUT_DIR
  - The remaining paths are extracted automatically.
3. Run the bash script.

Note:
- *It is important to extract the features for each system again*. So you need
to run the extractfeatures_single.sh file for each dataset on each system.
The features can vary very slightly from system to system.
- This should have no impact on the attribution and evasion, but the results will
be more interpretable, as you can see what features changed after a code transformation.

- **Do NOT use the already extracted features in ```data/dataset_2017/libtoolingfeatures_for_public_testing/```.**
They should only be used for the unit tests. For your own attribution
and evasion experiments, you need to extract the features yourself (as described above)!


### Getting started
- Load the python project *PyProject* into your IDE (e.g. PyCharm).
- Make sure your root directory is PyProject.
- Alternatively, you may need to set up the python path


If something goes wrong, check if all your paths are correctly set.
The directory ```/<path-to-repo>/data/dataset_2017/libtoolingfeatures_for_public_testing/```
has a symbolic link
```
/<path-to-repo>/data/dataset_2017/libtoolingfeatures_for_public_testing/dataset_2017_8_formatted_macrosremoved
-> /<path-to-repo>/data/dataset_2017/dataset_2017_8_formatted_macrosremoved
```
Check that this link is working on your OS. Moreover, check that all files are present.
Check you've called the file with the correct python path.
Otherwise, you can contact me if you cannot find the error.



### Some notes about the feature classes

-These are the features we extract-
  -Ast_node_bigram
  -Ast_node_types
  -Ast_node_max_depth
  -Code in Ast_leaves
  -Character 1-grams
  -Character 2-grams
  -Character 3-grams
  -Code2Vec embedding 

- *StyloFeatures*  is the abstract parent class.
- Then, we have a child class for the different sources of features.
We have features that we load from the java implementation by Caliskan et al.,
we have features that we can get from Python directly and we have features
that we obtain from our clang implementation (all the AST features).
- Features sources:
  1. *StyloARFFFeatures* contains the features that we load from the
  Java implementation by Caliskan et al..
    - It mostly contains layout features.
    - The lexical features that are present in this feature type
    are also present in our other feature sources. Therefore, you will often see
    that we do not load the ARFF features in our experiments.
    And that's why it is actually optional to set up all the java stuff.
    - Example: the number of functions is also given by an AST node type.
  2. Unigrams / Lexems:
    - To get unigram/lexem features, we have two implementations.
    - StyloUnigramFeatures loads the source code files in Python, and tokens
    are directly extracted in Python.
    - The second way loads the lexems as extracted by clang (look at the
    StyloLexemsFeaturesGenerator.py).
    - Both methods have almost the same output, and you can use the python
    version for convenience.
  3. *StyloClangFeaturesAbstract*:
    - Classes that inherit from this class contain all clang-based features,
    such as AST bigrams or *AST code in leaves* features.


- Another remark about the td-idf stuff: It is important that we perform
the tf-idf transformation only on the respective training set for any split
during the cross-validation. That's why we have the function *createtfidffeatures*.
If we have a training feature matrix, we call it without a trainingobject. Then,
we perform a tf-idf weighting on the training matrix. Later for the 'test'
matrix, we can simply pass the training object and we use the normalization
parameters from the training step for the test matrix as well. This avoids
data snooping.


## Copyright (c) 2022 

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (AuthAttLyzer), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
 
For citation in your works and also understanding AuthAttLyzer completely, you can find below published paper:

Abhishek Chopra and Arash Habibi Lashkari, "????", The second IEEE International Conference on Reconciling Data Analytics, Automation, Privacy, and Security: A Big Data Challenge (RDAAPS), Madrid, Spain, November 2022


## Project Team members 

* [**Arash Habibi Lashkari:**](http://ahlashkari.com/index.asp) Founder and supervisor

* [**Abhishek Chopra:**](https://github.com/abhishekchopra0907) Researcher and developer



### Acknowledgement 

This project has been made possible through funding from the [**Mitacs Global Internship**](https://www.mitacs.ca/en/programs/globalink/globalink-research-internship). 
