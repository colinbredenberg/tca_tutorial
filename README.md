# CIFAR-Helmholtz School Tutorial: Applying tensor decompositions to neural data

In this tutorial you will learn how tensor component analysis (TCA) works and how to implement it. We will finish by applying the TCA method to neural data, in order to analyze neural representations throughout learning.

## Setup

Before you come to the tutorial session, please try to download code and install the required packages in a virtual environment. If you have any questions, feel free to email Colin at: colinbre@uoregon.edu

If you are unable to download the code, don't worry! We will work through it on the day of the tutorial, and you will be fine as long as someone in your group has the code working.

As a preliminary step, ensure that you have cloned this Github repository to a folder on your computer, and have opened the folder in your IDE of choice. This code was developed and tested on a Mac with Python 3.13 installed, using VSCode as the IDE--you may have the best luck under these conditions. 

If you are using a Windows computer, I would recommend downloading either Windows Subsystem for Linux (WSL) or Git Bash, which will provide a similar command-line experience to working with the terminal on a Mac (based on UNIX). If you run into trouble, you can check out Chapter 2 of [this tutorial](https://wcresko.github.io/BioE_Comp_Tools_Fa2026/book/chapters/02-your-toolkit.html), which provides instructions for UNIX, Linux, and Windows for installing the basic components necessary to run this software. Alternatively, simply use the different required commands that are appropriate for the Windows shell.

Step 0: Navigate in your shell (command line) to a folder you would like the code to be, then download the code repository from Github using the following command.
```
git clone https://github.com/colinbredenberg/tca_tutorial.git
```


Step 1: create a python 3.13 virtual environment .venv in this repository folder

```
python -m venv .venv
```

Step 2: activate .venv
Mac/Linux
```
source .venv/bin/activate
```

Step 3: install required packages
```
pip install -r requirements.txt
```

## Instructions for downloading the dataset

We can save time on the day of the tutorial if you download the dataset that we will be working with in advance.

To download the data, first run the setup instructions above. Open `tutorial.ipynb` and select `.venv` as your kernel (to ensure that the IPython notebook has access to the packages we required in requirements.txt).

Finally, run the first two cells. The second cell will download a file `M1_LC_data.npy` into your code repo. It is ~400 Mb, so make sure you have space on your computer!