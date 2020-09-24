Title: Docker for Windows
Published: 09/04/2020
Tags:
    - chatbot
    - rasa
    - docker
    - windows
---

Rasa Chatbot Installtion Guide Step by Step for Windows Server
================================================================


1. Download and install Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017 and 2019


    [Download and instruction link](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)


2. Download and install anaconda for python distribution

    [https://www.anaconda.com/products/individual](https://www.anaconda.com/products/individual)

3. Create virtual environment using conda command prompt

    ```
    $ conda create -n bots python=3.7
	$ conda activate bots
	$ conda install python=3.6.5
	$ pip install tensorflow
    $ pip install rasa==2.0.0rc1
    ```

4. Create and directory for source and initiate rasa project creation

    ```
    e:
    mdkir rasaproj
    cd rasaproj
    rasa init
    ```
