# Install Chocolatey

This document will guide you through the installation of Chocolatey on Windows.

## Installation

1.  Open powershell as administrator.
2.  Run the following command:

        Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

## Install a package

Paste the following command in powershell:

    choco install <package_name>