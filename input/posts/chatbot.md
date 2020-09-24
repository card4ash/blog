Title: Rasa Chatbot Installation for Windows
Published: 09/24/2020
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

    check tensorflow installtion

    ```
    python -c 'import tensorflow as tf; print(tf.__version__)' 
    ```

4. Create and directory for source and initiate rasa project creation

    ```
    e:
    mdkir rasaproj
    cd rasaproj
    rasa init
    ```

5. Powershell Version Checking. Open Powershell and run the following to check the powershell version

    ```
        $PSVersionTable.PSVersion
    ```

6. If Powershell version lower than 5 then install using the following link

    [https://aka.ms/wmf5download](https://aka.ms/wmf5download)

7. For Installtin docker on Windows Server follow the below link

    [https://card4ash.github.io/blog/posts/docker](https://card4ash.github.io/blog/posts/docker)

8. For Install-PackageProvider : No match was found for the specified search criteria for the provider 'NuGet'. The package
provider requires 'PackageManagement' and 'Provider' tags. Please check if the specified package has the tags.
At line:1 char:1 related error try the following command

    ```
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
    ```

9. Get the latest version from PowerShell Gallery

    Before updating PowerShellGet, you should always install the latest NuGet provider. From an elevated PowerShell session, run the following command.

    ```
    Install-PackageProvider -Name NuGet -Force
    ```

10. For systems with PowerShell 5.0 (or newer) you can install the latest PowerShellGet

    To install PowerShellGet on Windows 10, Windows Server 2016, any system with WMF 5.0 or 5.1 installed, or any system with PowerShell 6, run the following commands from an elevated PowerShell session.

    ```
    Install-Module -Name PowerShellGet -Force
    ```

    Use Update-Module to get newer versions.

    ```
    Update-Module -Name PowerShellGet
    ```


links
=============

1. [https://docs.microsoft.com/en-us/powershell/scripting/gallery/installing-psget?view=powershell-7](https://docs.microsoft.com/en-us/powershell/scripting/gallery/installing-psget?view=powershell-7)


