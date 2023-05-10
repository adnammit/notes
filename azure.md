# Azure

## Why Azure?
* Azure is a cloud platform including infrastructure, platform and software as services (IaaS, PaaS, SaaS)
* azure is a lot of things, but using it for data storage in particular is a nice alternative to RDBMS as it allows vast quantities of data to be available with very low latency

## What Azure?
* **containers**: much like with docker, a container is a lightweight, immutable infrastructure for app packaging and developing
* **container registries** are used to deploy containers. they allow you to build, store, and manage container images and artifacts in a private registry for container deployment


## Azure Storage
* blob data can be accessed via the azure storage API, azure powershell and cli, or an azure storage client library such as `Azure.Storage.Blobs`
* **storage accounts** are the unique namespace for your data. the account name is used in the blob storage endpoint. for example, if your storage account is named `myaccount` the default endpoint for blob storage is `http://myaccount.blob.core.windows.net`
* **types of storage account**
	- **general purpose**
	- **block blob**
	- **page blob**
* **containers** organize a set of blobs. each account may have multiple containers containing many blobs. the uri for a container name `mycontainer` is `https://myaccount.blob.core.windows.net/mycontainer`
* **blobs** are the data! blobs are located at `https://myaccount.blob.core.windows.net/mycontainer/myblob` but you can also have virtual subdirectories such as `https://myaccount.blob.core.windows.net/mycontainer/myvirtualdirectory/myblob`
* **types of blob**:
	- **block blobs**: store text and binary data. blocks of data that can be managed individually
	- **append blobs**: like block blobs but optimized for append operations. ex: logging data from virtual machines
	- **page blobs**: random access files up to 8TB in size. these serve as disks for azure VMs


## Blobs and You
* **blobs** live in **containers** but can be downloaded to a local filesystem and treated like a regular file
* you can manage blobs and containers in several different ways:
	- [Azure CLI](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-cli)
	- [Azure Portal](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal)
	- [.NET Application using Azure.Storage.Blobs](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=net-cli)


### CLI Blob Management
* [manage blobs with the Azure CLI](https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/storage/blobs/blob-cli.md)
```
az login
az storage container list --account-name myaccount
```

## Identity and Authentication
* to access data in blob storage, you of course need to authenticate
* you could do this by providing an account access key, but that exposes risk as anyone with the access key can access all the data
* a better option is to use [`DefaultAzureCredential`](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/Identity-readme?view=azure-dotnet#defaultazurecredential) provided by Azure Identity
	- this supports multiple authentication methods and determines which to use at runtime
	- allows for use of different auth methods in different environments without having to write env-specific code
	- for example, you can authenticate locally through VS while developing, and then app can then use a **managed identity** once it is deployed
* applications should use service principals: see [service principals and managed identities](https://devblogs.microsoft.com/devops/demystifying-service-principals-managed-identities/)


## Environment and Deployment
* [setting environment variables](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-environment-variables)

