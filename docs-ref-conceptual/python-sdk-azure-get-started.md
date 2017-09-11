---
title: "用于 Python 的 Azure 库入门"
description: "用于 Python 的 Azure 库入门"
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: get-started
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 848ca9dc40000e68e5e3cea3af8b8a0d22747881
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-the-azure-libraries-for-python"></a><span data-ttu-id="ef10e-104">用于 Python 的 Azure 库入门</span><span class="sxs-lookup"><span data-stu-id="ef10e-104">Get started with the Azure libraries for Python</span></span>

<span data-ttu-id="ef10e-105">本指南演示多个用于 Python 的 Azure 库的用法。</span><span class="sxs-lookup"><span data-stu-id="ef10e-105">This guide demonstrates the usage of several Azure libraries for Python.</span></span>  <span data-ttu-id="ef10e-106">内容包括设置身份验证、创建和使用 Azure 存储帐户、创建和使用 Azure SQL 数据库、部署一些虚拟机，然后从 GitHub 部署一个 Azure 应用服务 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="ef10e-106">You will set up authentication, create and use an Azure Storage account, create and use an Azure SQL Database, deploy some virtual machines, and deploy an Azure App Service Web App from GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef10e-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="ef10e-107">Prerequisites</span></span>

- <span data-ttu-id="ef10e-108">一个 Azure 帐户。</span><span class="sxs-lookup"><span data-stu-id="ef10e-108">An Azure account.</span></span> <span data-ttu-id="ef10e-109">如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)。</span><span class="sxs-lookup"><span data-stu-id="ef10e-109">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
- [<span data-ttu-id="ef10e-110">Python</span><span class="sxs-lookup"><span data-stu-id="ef10e-110">Python</span></span>](https://www.python.org/downloads/)
- <span data-ttu-id="ef10e-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) 或 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2)。</span><span class="sxs-lookup"><span data-stu-id="ef10e-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

[!INCLUDE [azure-cloud-shell](../docs-ref-conceptual/includes/cloud-shell-try-it.md)]

## <a name="set-up-authentication"></a><span data-ttu-id="ef10e-112">设置身份验证</span><span class="sxs-lookup"><span data-stu-id="ef10e-112">Set up authentication</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ef10e-113">应将此方法用作开发人员快速入门体验。</span><span class="sxs-lookup"><span data-stu-id="ef10e-113">This should be used as quick start developer experience.</span></span> <span data-ttu-id="ef10e-114">对于生产用途，请使用 [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) 或自己的凭据系统。</span><span class="sxs-lookup"><span data-stu-id="ef10e-114">For production purposes, use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) or your own credentials system.</span></span>
> <span data-ttu-id="ef10e-115">对 CLI 配置进行任何更改会影响 SDK 的执行。</span><span class="sxs-lookup"><span data-stu-id="ef10e-115">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="ef10e-116">SDK 能够使用 CLI 活动订阅创建客户端。</span><span class="sxs-lookup"><span data-stu-id="ef10e-116">The SDK is able to create a client using your CLI active subscription.</span></span>

<span data-ttu-id="ef10e-117">若要定义活动的凭据，请使用 [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli)。</span><span class="sxs-lookup"><span data-stu-id="ef10e-117">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="ef10e-118">默认的订阅 ID 是你拥有的唯一 ID，或使用 [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) 定义的 ID。</span><span class="sxs-lookup"><span data-stu-id="ef10e-118">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli).</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
## <a name="create-a-virtual-environment"></a><span data-ttu-id="ef10e-119">创建虚拟环境</span><span class="sxs-lookup"><span data-stu-id="ef10e-119">Create a virtual environment</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef10e-120">创建虚拟环境是可选的操作，但我们强烈建议这样做。</span><span class="sxs-lookup"><span data-stu-id="ef10e-120">Create a virtual environment is optional, but we strongly suggest you to do it.</span></span>

<span data-ttu-id="ef10e-121">在 Bash 中创建虚拟环境（Linux 或 [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about)）：</span><span class="sxs-lookup"><span data-stu-id="ef10e-121">Create a virtual environment in Bash (Linux or [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about)):</span></span>
```bash
pip install virtualenv
virtualenv mytestenv
cd mytestenv
source ./bin/activate
```

<span data-ttu-id="ef10e-122">在 Powershell 中创建虚拟环境：</span><span class="sxs-lookup"><span data-stu-id="ef10e-122">Create a virtual environment in Powershell:</span></span>
```powershell
pip install virtualenv
virtualenv mytestenv
cd mytestenv
.\Scripts\activate
```

> [!IMPORTANT]
> <span data-ttu-id="ef10e-123">请注意，如果使用 [VSCode](https://code.visualstudio.com/) 和 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python)，可以使用 `code .` 来启动它并配置环境。</span><span class="sxs-lookup"><span data-stu-id="ef10e-123">Note that if you use [VSCode](https://code.visualstudio.com/) and the [Python extension](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python),  you can start it using `code .` and get your environment configured.</span></span>

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="ef10e-124">创建 Linux 虚拟机</span><span class="sxs-lookup"><span data-stu-id="ef10e-124">Create a Linux virtual machine</span></span>
<span data-ttu-id="ef10e-125">此代码会在美国东部 Azure 区域中运行的资源组 `sampleVmResourceGroup` 内创建名为 `testLinuxVM` 的新 Linux VM。</span><span class="sxs-lookup"><span data-stu-id="ef10e-125">This code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleVmResourceGroup` running in the US East Azure region.</span></span>

<span data-ttu-id="ef10e-126">身份验证：</span><span class="sxs-lookup"><span data-stu-id="ef10e-126">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="ef10e-127">创建资源组：</span><span class="sxs-lookup"><span data-stu-id="ef10e-127">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus --n sampleVmResourceGroup
```

<span data-ttu-id="ef10e-128">创建虚拟网络和子网：</span><span class="sxs-lookup"><span data-stu-id="ef10e-128">Create a virtual network and subnet:</span></span>
```azurecli-interactive
az network vnet create -g sampleVmResourceGroup -n azure-sample-vnet --address-prefix 10.0.0.0/16 --subnet-name azure-sample-subnet --subnet-prefix 10.0.0.0/24
```

<span data-ttu-id="ef10e-129">创建公共 IP 地址：</span><span class="sxs-lookup"><span data-stu-id="ef10e-129">Create a public IP address:</span></span>
```azurecli-interactive
az network public-ip create -g sampleVmResourceGroup -n azure-sample-ip --allocation-method Dynamic --version IPv6
```
<span data-ttu-id="ef10e-130">创建网络接口客户端：</span><span class="sxs-lookup"><span data-stu-id="ef10e-130">Create a network interface client:</span></span>
```azurecli-interactive
az network nic create -g sampleVmResourceGroup --vnet-name azure-sample-vnet --subnet azure-sample-subnet -n azure-sample-nic --public-ip-address azure-sample-ip
```

```python
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient
from azure.common.client_factory import get_client_from_cli_profile

# Azure Datacenter
LOCATION = 'eastus'

# Resource Group
GROUP_NAME = 'sampleVmResourceGroup'

# Network
VNET_NAME = 'azure-sample-vnet'
SUBNET_NAME = 'azure-sample-subnet'

# VM
NIC_NAME = 'azure-sample-nic'
VM_NAME = 'testLinuxVM'

USERNAME = 'userlogin'
PASSWORD = 'Pa$$w0rd91'

IP_ADDRESS_NAME='azure-sample-ip'

VM_REFERENCE = {
    'linux': {
        'publisher': 'Canonical',
        'offer': 'UbuntuServer',
        'sku': '16.04.0-LTS',
        'version': 'latest'
    },
    'windows': {
        'publisher': 'MicrosoftWindowsServerEssentials',
        'offer': 'WindowsServerEssentials',
        'sku': 'WindowsServerEssentials',
        'version': 'latest'
    }
}


def run_example():

    compute_client = get_client_from_cli_profile(ComputeManagementClient)
    network_client = get_client_from_cli_profile(NetworkManagementClient)

    # get nic id
    nic = network_client.network_interfaces.get(GROUP_NAME, NIC_NAME)

    # Create Linux VM
    print('\nCreating Linux Virtual Machine')
    vm_parameters = create_vm_parameters(nic.id, VM_REFERENCE['linux'])
    print(vm_parameters)
    async_vm_creation = compute_client.virtual_machines.create_or_update(
        GROUP_NAME, VM_NAME, vm_parameters)
    async_vm_creation.wait()


def create_vm_parameters(nic_id, vm_reference):
    """Create the VM parameters structure.
    """
    return {
        'location': LOCATION,
        'os_profile': {
            'computer_name': VM_NAME,
            'admin_username': USERNAME,
            'admin_password': PASSWORD
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': vm_reference['publisher'],
                'offer': vm_reference['offer'],
                'sku': vm_reference['sku'],
                'version': vm_reference['version']
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': nic_id,
            }]
        },
    }


if __name__ == "__main__":
    run_example()
```

<span data-ttu-id="ef10e-131">程序完成后，使用 Azure CLI 2.0 验证订阅中的虚拟机：</span><span class="sxs-lookup"><span data-stu-id="ef10e-131">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="ef10e-132">验证代码正常运行后，使用 CLI 删除 VM 及其资源。</span><span class="sxs-lookup"><span data-stu-id="ef10e-132">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="ef10e-133">从 GitHub 存储库部署 Web 应用</span><span class="sxs-lookup"><span data-stu-id="ef10e-133">Deploy a web app from a GitHub repo</span></span>
<span data-ttu-id="ef10e-134">此代码会将 GitHub 存储库的 `master` 分支中的某个 Flask Web 应用程序部署到免费层中运行的新 [Azure 应用服务 Web 应用](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview)。</span><span class="sxs-lookup"><span data-stu-id="ef10e-134">This code deploys a Flask web application from the `master` branch in a GitHub repo in to a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free tier.</span></span> 

<span data-ttu-id="ef10e-135">开始之前：创建 https://github.com/Azure-Samples/python-docs-hello-world 的分叉</span><span class="sxs-lookup"><span data-stu-id="ef10e-135">Before you begin: Fork https://github.com/Azure-Samples/python-docs-hello-world</span></span>

<span data-ttu-id="ef10e-136">身份验证：</span><span class="sxs-lookup"><span data-stu-id="ef10e-136">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="ef10e-137">创建资源组：</span><span class="sxs-lookup"><span data-stu-id="ef10e-137">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus -n sampleWebResourceGroup
```

<span data-ttu-id="ef10e-138">创建免费应用服务计划：</span><span class="sxs-lookup"><span data-stu-id="ef10e-138">Create a free app service plan:</span></span>
```azurecli-interactive
az appservice plan create -g sampleWebResourceGroup -n sampleFreePlan  --sku Free
```

```python
from azure.mgmt.web import WebSiteManagementClient
from azure.mgmt.web.models import Site, SiteSourceControl, SiteConfig
from azure.common.client_factory import get_client_from_cli_profile

RESOURCE_GROUP_NAME = 'sampleWebResourceGroup'
SERVICE_PLAN_NAME = 'sampleFreePlanName'
WEB_APP_NAME = 'sampleflaskapp123'
REPO_URL = 'GitHub_Repository_URL'

#log in
web_client = get_client_from_cli_profile(WebSiteManagementClient)

# get service plan id
service_plan = web_client.app_service_plans.get(RESOURCE_GROUP_NAME, SERVICE_PLAN_NAME)

# create a web app
siteConfiguration = SiteConfig(
    python_version='3.4',
)
site_async_operation = web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=service_plan.id,
        site_config=siteConfiguration
    ),
)

site = site_async_operation.result()
print('created webapp: ' + site.default_host_name)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url= REPO_URL,
        branch='master'
    )
)

source_control = source_control_async_operation.result()
print("added source control to: " + source_control.name + "azurewebsites.net")
```

<span data-ttu-id="ef10e-139">使用 CLI 打开指向该应用程序的浏览器：</span><span class="sxs-lookup"><span data-stu-id="ef10e-139">Open a browser pointed to the application using the CLI:</span></span>
```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

<span data-ttu-id="ef10e-140">验证部署后，请从订阅中删除 Web 应用和计划。</span><span class="sxs-lookup"><span data-stu-id="ef10e-140">Remove the web app and plan from your subscription once you've verified the deployment.</span></span> 
```azurecli-interactive
az group delete --name sampleWebResourceGroup
```


## <a name="connect-to-a-sql-database"></a><span data-ttu-id="ef10e-141">连接到 SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="ef10e-141">Connect to a SQL database</span></span>
<span data-ttu-id="ef10e-142">此代码会创建新的 SQL 数据库（包含一条允许远程访问的防火墙规则），然后使用 Microsoft ODBC 驱动程序连接到该数据库。</span><span class="sxs-lookup"><span data-stu-id="ef10e-142">This code creates a new SQL database with a firewall rule allowing remote access, and then connected to it using the Microsoft ODBC driver.</span></span> <span data-ttu-id="ef10e-143">Pyodbc 使用 Linux 上的 Microsoft ODBC 驱动程序连接到 SQL 数据库。</span><span class="sxs-lookup"><span data-stu-id="ef10e-143">Pyodbc uses the Microsoft ODBC Driver on Linux to connect to SQL Databases.</span></span> 

<span data-ttu-id="ef10e-144">身份验证：</span><span class="sxs-lookup"><span data-stu-id="ef10e-144">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="ef10e-145">创建资源组：</span><span class="sxs-lookup"><span data-stu-id="ef10e-145">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus -n azure-sample-group
```

<span data-ttu-id="ef10e-146">创建 SQL 服务器：</span><span class="sxs-lookup"><span data-stu-id="ef10e-146">Create a SQL server:</span></span>
```azurecli-interactive
az sql server create --admin-password HusH_Sec4et --admin-user mysecretname -l eastus -n samplesqlserver123 -g azure-sample-group
```

<span data-ttu-id="ef10e-147">添加防火墙规则：</span><span class="sxs-lookup"><span data-stu-id="ef10e-147">Add firewall rule:</span></span>
```azurecli-interactive
az sql server firewall-rule create --end-ip-address 167.220.0.235 --name firewall_rule_name_123.123.123.123 --resource-group azure-sample-group --server samplesqlserver123 --start-ip-address 123.123.123.123
```

<span data-ttu-id="ef10e-148">创建 SQL 数据库：</span><span class="sxs-lookup"><span data-stu-id="ef10e-148">Create a SQL database:</span></span>
```azurecli-interactive
az sql db create --name sample-db --resource-group azure-sample-group --server samplesqlserver123
```


```python
from azure.mgmt.sql import SqlManagementClient
from azure.common.client_factory import get_client_from_cli_profile
import pyodbc

LOCATION = 'eastus'
GROUP_NAME = 'azure-sample-group'
SERVER_NAME = 'samplesqlserver123'
DATABASE_NAME = 'sample-db'
USER_NAME ='mysecretname'
PASSWORD='HusH_Sec4et'

# authenticate
sql_client = get_client_from_cli_profile(SqlManagementClient)


def run_example():
    # Get SQL database
    print('Get SQL database')
    database = sql_client.databases.get(
        GROUP_NAME,
        SERVER_NAME,
        DATABASE_NAME
    )
    print(database)
    print("\n\n")


def create_and_insert():
    server = SERVER_NAME+'.database.windows.net'
    database = DATABASE_NAME
    username = USER_NAME
    password = PASSWORD
    driver = '{ODBC Driver 13 for SQL Server}'
    cnxn = pyodbc.connect(
        'DRIVER=' + driver + ';PORT=1433;SERVER=' + server + ';PORT=1443;DATABASE=' + database + ';UID=' + username + ';PWD=' + password)
    cursor = cnxn.cursor()
    tsql = "CREATE TABLE CLOUD (name varchar(255), code int);"
    tsqlInsert = "INSERT INTO CLOUD (name, code) VALUES ('Azure', 1); "
    selectValues = "SELECT * FROM CLOUD"
    with cursor.execute(tsql):
        print('Successfully created table!')
    with cursor.execute(tsqlInsert):
        print('Successfully inserted data into table')
    cursor.execute(selectValues)
    row = cursor.fetchone()
    while row:
        print(str(row[0]) + " " + str(row[1]))
        row = cursor.fetchone()
    cnxn.commit()

if __name__ == "__main__":
    run_example()
    create_and_insert()
```

<span data-ttu-id="ef10e-149">验证代码正常运行后，使用 CLI 删除 SQL 数据库及其资源。</span><span class="sxs-lookup"><span data-stu-id="ef10e-149">Once you've verified that the code worked, use the CLI to delete the SQL database and its resources.</span></span>

```azurecli-interactive
az group delete --name azure-sample-group
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="ef10e-150">将 Blob 写入新存储帐户</span><span class="sxs-lookup"><span data-stu-id="ef10e-150">Write a blob into a new storage account</span></span>

<span data-ttu-id="ef10e-151">身份验证：</span><span class="sxs-lookup"><span data-stu-id="ef10e-151">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="ef10e-152">创建资源组：</span><span class="sxs-lookup"><span data-stu-id="ef10e-152">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus -n sampleStorageResourceGroup
```

<span data-ttu-id="ef10e-153">创建存储帐户：</span><span class="sxs-lookup"><span data-stu-id="ef10e-153">Create a storage account:</span></span>
```azurecli-interactive
az storage account create -n samplestorageaccountname -g sampleStorageResourceGroup -l eastus --sku Standard_RAGRS
```

<span data-ttu-id="ef10e-154">此代码会创建一个 [Azure 存储帐户](https://docs.microsoft.com/azure/storage/storage-introduction)，然后使用用于 Python 的 Azure 存储库在云中创建新的 html 文件。</span><span class="sxs-lookup"><span data-stu-id="ef10e-154">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Python to create a new html file in the cloud.</span></span> 
```python
from azure.storage import CloudStorageAccount
from azure.storage.blob import PublicAccess
from azure.storage.blob.models import ContentSettings
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.storage import StorageManagementClient

RESOURCE_GROUP = 'sampleStorageResourceGroup'
STORAGE_ACCOUNT_NAME = 'samplestorageaccountname'
CONTAINER_NAME = 'samplecontainername'

# log in
storage_client = get_client_from_cli_profile(StorageManagementClient)

# create a public storage container to hold the file
storage_keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP, STORAGE_ACCOUNT_NAME)
storage_keys = {v.key_name: v.value for v in storage_keys.keys}

storage_client = CloudStorageAccount(STORAGE_ACCOUNT_NAME, storage_keys['key1'])
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(CONTAINER_NAME, public_access=PublicAccess.Container)

blob_service.create_blob_from_bytes(
    CONTAINER_NAME,
    'helloworld.html',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url(CONTAINER_NAME, 'helloworld.html'))
```
<span data-ttu-id="ef10e-155">若要内容是否成功上传，请导航到列显的 URL。</span><span class="sxs-lookup"><span data-stu-id="ef10e-155">To verify content successfully uploaded, navigate to the url printed.</span></span> 

<span data-ttu-id="ef10e-156">使用 CLI 清理存储帐户：</span><span class="sxs-lookup"><span data-stu-id="ef10e-156">Clean up the storage account using the CLI:</span></span>
```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="ef10e-157">学习更多示例</span><span class="sxs-lookup"><span data-stu-id="ef10e-157">Explore more samples</span></span>

<span data-ttu-id="ef10e-158">若要详细了解如何使用用于 Python 的 Azure 管理库来管理资源和自动执行任务，请参阅针对[虚拟机](python-sdk-azure-web-apps-samples.md)、[Web 应用](python-sdk-azure-web-apps-samples.md)和 [SQL 数据库](python-sdk-azure-sql-database-samples.md)的示例代码。</span><span class="sxs-lookup"><span data-stu-id="ef10e-158">To learn more about how to use the Azure management libraries for Python to manage resources and automate tasks, see our sample code for [virtual machines](python-sdk-azure-web-apps-samples.md), [web apps](python-sdk-azure-web-apps-samples.md) and [SQL database](python-sdk-azure-sql-database-samples.md).</span></span>


## <a name="reference"></a><span data-ttu-id="ef10e-159">引用</span><span class="sxs-lookup"><span data-stu-id="ef10e-159">Reference</span></span>

<span data-ttu-id="ef10e-160">我们为所有包提供了[参考](/python/api/overview/azure/?view=azure-python)文档。</span><span class="sxs-lookup"><span data-stu-id="ef10e-160">A [reference](/python/api/overview/azure/?view=azure-python) is available for all packages.</span></span>
