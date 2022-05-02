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
Read the [attribution-markdown](./README_ATTRIBUTION.md) file to get the details how to set up the whole attribution.

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
