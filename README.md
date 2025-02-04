# codespaces
Base Template for codespaces

## Using GitHub Codespaces for Security Testing on Microsoft Azure

GitHub Codespaces provides a powerful and flexible environment for developing and testing applications. It can also be used as an interface to perform security testing on Microsoft Azure. This section will guide you through the steps to set up your Codespace environment and provide examples of security testing tools and commands.

### Setting Up the Codespace Environment

1. **Create a GitHub Codespace**: 
   - Navigate to your repository on GitHub.
   - Click on the `Code` button and select `Open with Codespaces`.
   - If you don't have any existing Codespaces, click on `New codespace`.

2. **Configure the `.devcontainer/devcontainer.json` file**:
   - Ensure that your `devcontainer.json` file includes the necessary extensions and tools for security testing. Here is an example configuration:

     ```json
     {
       "name": "BlackCat",
       "image": "mcr.microsoft.com/azure-powershell:latest",
       "customizations": {
         "vscode": {
           "settings": {
             "debug.javascript.terminal.activateWithCurrentFile": false,
             "debug.javascript.autoAttachFilter": "disabled",
             "debug.javascript.automaticallyTunnelRemoteServer": false,
             "terminal.integrated.profiles.linux": {
               "bash": {
                 "path": "bash",
                 "icon": "terminal-bash"
               },
               "BlackCat": {
                 "path": "/opt/microsoft/powershell/7/pwsh",
                 "icon": "terminal-powershell"
               }
             },
             "terminal.integrated.defaultProfile.linux": "BlackCat"
           },
           "extensions": [
             "ms-vscode.powershell",
             "ms-azuretools.vscode-bicep",
             "GitHub.copilot",
             "shardulm94.trailing-spaces",
             "GitHub.vscode-github-actions",
             "DavidAnson.vscode-markdownlint",
             "ms-vscode.vscode-node-azure-pack",
             "ms-azuretools.vscode-azureterraform"
           ]
         }
       },
       "features": {
         "ghcr.io/devcontainers/features/common-utils:2": {
           "installZsh": "false",
           "username": "vscode",
           "upgradePackages": "true",
           "nonFreePackages": "false"
         },
         "ghcr.io/devcontainers/features/azure-cli:1": {
           "installBicep": "true",
           "installUsingPython": "true"
         }
       },
       "postCreateCommand": "mkdir /root/.config/powershell && echo 'Import-Module -Name ./src/BlackCat.psd1' > /root/.config/powershell/Microsoft.PowerShell_profile.ps1 && ./.devcontainer/update.sh && Export-AccessTokens && Install-Module -Name AzSK -Scope CurrentUser -AllowClobber -Force"
     }
     ```

### Examples of Security Testing Tools and Commands

1. **AzSK (Azure Security Kit)**:
   - AzSK is a tool that provides security verification controls and scripts for Azure resources.
   - To install AzSK, run the following command in your Codespace terminal:
     ```powershell
     Install-Module -Name AzSK -Scope CurrentUser -AllowClobber -Force
     ```
   - To run a security scan on your Azure subscription, use the following command:
     ```powershell
     Get-AzSKSubscriptionSecurityStatus -SubscriptionId <Your-Subscription-Id>
     ```

2. **Azure CLI Security Extensions**:
   - Azure CLI can be extended with additional security-related commands.
   - To install the Azure CLI security extension, run the following command:
     ```bash
     az extension add --name security
     ```
   - To check the security status of your Azure resources, use the following command:
     ```bash
     az security assessment list
     ```

By following these steps and using the provided tools and commands, you can effectively perform security testing on Microsoft Azure using GitHub Codespaces.
