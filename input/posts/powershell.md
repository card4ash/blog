Title: Powershell fundamentals
Published: 10/06/2020
Tags:
    - powershell
    - windows
---

1. Powershell Version Checking. Open Powershell and run the following to check the powershell version

    ```
        $PSVersionTable.PSVersion
    ```

2. If Powershell version lower than 5 then install using the following link

    [https://aka.ms/wmf5download](https://aka.ms/wmf5download)

3. For Installtin docker on Windows Server follow the below link

    [https://card4ash.github.io/blog/posts/docker](https://card4ash.github.io/blog/posts/docker)

4. For Install-PackageProvider : No match was found for the specified search criteria for the provider 'NuGet'. The package
provider requires 'PackageManagement' and 'Provider' tags. Please check if the specified package has the tags.
At line:1 char:1 related error try the following command

    ```
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
    ```

5. Get the latest version from PowerShell Gallery

    Before updating PowerShellGet, you should always install the latest NuGet provider. From an elevated PowerShell session, run the following command.

    ```
    Install-PackageProvider -Name NuGet -Force
    ```

6. For systems with PowerShell 5.0 (or newer) you can install the latest PowerShellGet

    To install PowerShellGet on Windows 10, Windows Server 2016, any system with WMF 5.0 or 5.1 installed, or any system with PowerShell 6, run the following commands from an elevated PowerShell session.

    ```
    Install-Module -Name PowerShellGet -Force
    ```

    Use Update-Module to get newer versions.

    ```
    Update-Module -Name PowerShellGet
    ```

7. Install module packagemanagement for powershell

    ```
    Install-Module -Name PackageManagement
    ```

8. Powershell Repository

    ```
    Get-PSRepository
    ```


links
=============

1. [https://docs.microsoft.com/en-us/powershell/scripting/gallery/installing-psget?view=powershell-7](https://docs.microsoft.com/en-us/powershell/scripting/gallery/installing-psget?view=powershell-7)
