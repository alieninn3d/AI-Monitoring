============================================================================
# Ai Monitoring
============================================================================

## This project serves two purposes.

The first being learn and document the practices required in Microsoft Azure, Microsoft Azure DevOps, and Microsoft Azure Artificial Intelligence as the project progresses.

The second purpose is to develop and publish the Alien Inn Pty Ltd’s artificial intelligence monitoring platform that will be used to monitor ongoing WWW.AI2D.AI projects planned for the future.

## Project Restrictions

1.	The project is to utilise SquaredUp Community Edition as the monitoring solution
2.	The solution is to utilise four (4) Microsoft Server 2019 or later based IIS servers as a clustered platform.
3.	A dedicated shared disk will be utilised.
4.	A Ubuntu server will be deployed under the same Azure resource group with Time Server set as Stratum Two.

## Project DONE-DONE

Project is "done" when the solution publishes server performance data to the SquredUp interface and is visible at www.monitoring.alien-inn.com.au 

============================================================================
## Document Standards

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)

============================================================================
### Support or Contact

- Google is your friend
- There is much to learn
============================================================================
# SECTION
============================================================================


============================================================================
PROJECT BUILD PROCESS NOTES
============================================================================

1. Create a Microsoft Account to use with Azure, Azsure DevOps, and Azure AI
2. Setup your Windows 10 Development system
2.1 Microsoft Visual Studio (version)
2.2 Microsoft Azure CLI
2.3 Microsoft Code


============================================================================
REFERENCES
============================================================================




============================================================================
PROJECT PROGRESS NOTES
============================================================================






and now Visual Studio 2019 is syncronised with GitHub repositories

============================================================================

EDIT 22/10/2021:
Gathering notes and decyphering Microsoft Tutorials

az login
# https://adamtheautomator.com/azure-cli/

Connect-AzAccount
az account list
#az configure --defaults location=westus2 group=MyResourceGroup
az account set --subscription "Microsoft Partner Network"
#az extension add --name azure-devops
#az devops project list --org https://dev.azure.com/alieninndevelopment -properties *
az devops project list --org https://dev.azure.com/alieninndevelopment --query "value[].name" #-o txt

# create some stuff following this https://docs.microsoft.com/en-us/cli/azure/azure-cli-vm-tutorial?tutorial-step=1
az vm image list
az group create --name AI-Monitoring-ResourceGroup --location eastus # in reality this should sit in Australia but for now, US is ok
# install OpenSSH Server on windows 10 using Add Feature following this https://www.partitionwizard.com/partitionmagic/ssh-client-win10.html

az vm create --resource-group AI-Monitoring-ResourceGroup --name AITIMESOURCE01 --image UbuntuLTS --generate-ssh-keys --output json --verbose
az vm list-ip-addresses --resource-group AI-Monitoring-ResourceGroup

#ssh azureuser@52.169.255.149

az vm show --name AITIMESOURCE01 --resource-group AI-Monitoring-ResourceGroup
az vm show --resource-group AI-Monitoring-ResourceGroup --name AITIMESOURCE01

az vm show --name AITIMESOURCE01 --resource-group AI-Monitoring-ResourceGroup --query 'networkProfile.networkInterfaces[].id' -o tsv

$NIC_ID = $(az vm show -n AITIMESOURCE01 -g AI-Monitoring-ResourceGroup --query 'networkProfile.networkInterfaces[].id' -o tsv)

az network nic show --ids $NIC_ID
az network nic show --ids $NIC_ID --query '{IP:ipConfigurations[].publicIpAddress.id, Subnet:ipConfigurations[].subnet.id}' -o tsv
az vm show -d -g AI-Monitoring-ResourceGroup -n AITIMESOURCE01 --query publicIps -o tsv

# set some more variables

$IP_ID = $(az network nic show --ids $NIC_ID --query '{IP:ipConfigurations[].publicIpAddress.id}' -o tsv)
$SUBNET_ID = $(az network nic show --ids $NIC_ID --query '{IP:ipConfigurations[].subnet.id]}' -o tsv)

$VM1_IP_ADDR = $(az network public-ip show --ids $IP_ID --query ipAddress -o tsv)


read -d '' IP_ID SUBNET_ID <<< $(az network nic show --ids $NIC_ID --query '[ipConfigurations[].publicIpAddress.id, ipConfigurations[].subnet.id]' -o tsv)



#az devops configure defaults --
# downlaod from: https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli 
# https://docs.microsoft.com/en-us/azure/devops/cli/?view=azure-devops
# 



