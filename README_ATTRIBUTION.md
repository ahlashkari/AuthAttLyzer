# Authorship Attribution
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
  - Adjust the path to the java jar.
    I've used an environment variable CMD_DIR_NAIVEBASELINEJAR but you can set the path here simply.
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
- Alternatively, you may need to set up the python path. Or if you're in your terminal,
you can set the path for each python call, such as:
```
$PYPATH="<path-to-repo>"
$PYTHONPATH=${PYPATH} python evaluations/blackbox/attack/blackbox_attack_batch.py
```

### Test your setup
1. Open the python file *UnitTests/testFeatureExctraction.py*. All tests should pass.
2. Open the python file *UnitTests/test_learning.py*. All tests should pass.

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
