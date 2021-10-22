## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/alieninn3d/AI-Monitoring/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/alieninn3d/AI-Monitoring/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.



==================================================================================================
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



