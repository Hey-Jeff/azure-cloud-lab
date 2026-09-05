# ☁️ Lab 01 — Understanding Azure Fundamentals

## 🎯 Goal

The goal of this lab is to understand how Microsoft Azure organizes cloud resources before starting to Deploy | Create virtual machines, networks, storage, or Kubernetes.

In this lab you will learn:

- What an Azure Tenant is
- What an Azure Subscription is
- What a Resource Group is
- How Azure resources are organized
- How to create a Resource Group
- How to interact with Azure using the Azure CLI

---

# 🧠 First: How does Azure organize resources?

Before creating cloud infrastructure, it is important to understand where resources actually live.

A simplified Azure hierarchy looks like this:

```text
Azure
│
└── Tenant
     │
     └── Subscription
          │
          └── Resource Group
               │
               ├── Virtual Machine(resource)
               ├── Virtual Network(resource)
               ├── Storage(resource)
               └── Kubernetes Cluster(resource)
```

Each level has a different purpose.

---

# 🏢 What is an Azure Tenant?

An Azure Tenant represents an organization's Microsoft Entra ID(Cloud Active Directory) environment.

It is responsible for identity.

For example, it can contain:

- Users
- Groups
- Applications
- Authentication settings
- Permissions

A simple way to think about the Tenant is:

> The organization that owns and manages the identities using Azure.

---

# 💳 What is an Azure Subscription?

A Subscription is where Azure resources are created and billed.

It provides a boundary for things such as:

- Billing
- Resource usage
- Access control
- Service limits

A simple way to think about it is:

> If Azure is the cloud platform, the Subscription is the account that pays for the resources I use or the company uses.

---

# 📦 What is a Resource Group?

A Resource Group is a logical container used to organize Azure resources.

For example:

```text
rg-COMPANY-cloud-lab

├── Virtual Network (resource)
├── Virtual Machine (resource)
├── Storage Account (resource)
└── AKS Cluster (resource)
```

The Resource Group does not run applications by itself.

Its job is to organize related resources.

---

# 🏠 Simple Analogy

Imagine an apartment building.

```text
Azure Tenant
    │
    └── Apartment Building | Condo "The Enclaves"

Azure Subscription
    │
    └── My contract/account

Resource Group
    │
    └── My apartment # 4141

Azure Resources | Resources in my apartment
    │
    ├── TV
    ├── Computer
    ├── Furniture
    └── Lights
```

In Azure, those objects could instead be:

```text
Virtual Machine
Virtual Network
Storage Account
Database
AKS Cluster
```

The analogy is not technically exact, but it helps visualize how resources are grouped.

---

# 🧪 What am I building?

For this cloud lab I created the following Resource Group:

```text
rg-jeffsApartment-cloud-lab
```

Future resources for this project will be organized inside this Resource Group.

Eventually it may contain resources such as:

```text
rg-jeffsApartment-cloud-lab
│
├── vnet-jeffry-lab
├── nsg-jeffry-lab
├── vm-linux-lab
├── storage
└── aks-jeffry-lab
```

---

# 🖥️ Creating the Resource Group using Azure Portal, for reference: https://azure.microsoft.com/en-us/get-started/azure-portal

I first created the Resource Group using the Azure Portal.

Steps:

1. Open Azure Portal
2. Search for **Resource Groups**
3. Select **Create**
4. Choose the Azure Subscription
5. Enter the Resource Group name

```text
rg-jeffsApartment-cloud-lab
```

6. Choose an Azure region
7. Select **Review + Create**
8. Create the Resource Group

This demonstrates how Azure resources can be managed using the graphical interface.

---

# 💻 Azure CLI

Azure resources can also be managed from the command line.

This is useful because Cloud Engineers often manage infrastructure using commands and automation instead of manually clicking through the Azure portal.

---

## Check the current Azure account

```bash
az account show
```

### What does this command do?

`az` tells the system to use the Azure CLI.

`account` means we want information about the Azure account.

`show` means display the currently active account.

So:

```bash
az account show
```

basically means:

> Show me which Azure subscription and account I am currently using.

---

# 📋 List Resource Groups

```bash
az group list --output table
```

Breaking the command down:

```text
az
```

Use Azure CLI.

```text
group
```

Work with Resource Groups.

```text
list
```

Show the Resource Groups.

```text
--output table
```

Display the result in an easy-to-read table.

So the command means:

> Show me all Resource Groups available in my current Azure subscription.

---

# ➕ Create a Resource Group using Azure CLI

```bash
az group create \
  --name rg-jeffry-cli-lab \
  --location eastus
```

Breaking it down:

```text
az group create
```

Create a new Resource Group.

```text
--name
```

Specify the name.

```text
rg-jeffry-cli-lab
```

The name of the Resource Group.

```text
--location eastus
```

Store the Resource Group metadata in the East US Azure region.

---

# ✅ Verify the Resource Group

After creating it, you can run:

```bash
az group list --output table
```

I should now see:

```text
rg-jeffry-cli-lab
```

in the list.

This is an important part of cloud engineering:

> Never assume that a deployment worked. Always verify it.

---

# 🗑️ Delete the temporary Resource Group

Because this Resource Group was created only for practice:

```bash
az group delete \
  --name rg-jeffry-cli-lab \
  --yes \
  --no-wait
```

### What does this mean?

```text
az group delete
```

Delete a Resource Group.

```text
--name
```

Specify which Resource Group.

```text
--yes
```

Automatically confirm the deletion.

```text
--no-wait
```

Return control of the terminal without waiting for Azure to completely finish the deletion.

---

# ⚠️ Important Lesson

Deleting a Resource Group can also delete the resources contained inside it.

For example:

```text
rg-production
│
├── Virtual Machine
├── Database
├── Storage
└── Network
```

Deleting the Resource Group could remove the entire environment.

This is why Resource Group deletion must be handled carefully in production environments.

---

# 🔎 Troubleshooting

## Problem: I cannot see the Resource Group I created

First check which Azure account is active:

```bash
az account show
```

Then list available subscriptions:

```bash
az account list --output table
```

It is possible that the Resource Group was created in another subscription.

---

## Problem: Azure CLI commands are failing

Verify that Azure CLI is authenticated.

```bash
az login
```

Then confirm the active account:

```bash
az account show
```

---

# 🧹 Cleanup

The temporary CLI Resource Group was deleted after completing the exercise.

The main Resource Group remains available for future labs:

```text
rg-jeffsApartment-cloud-lab
```

Future Azure networking, compute, monitoring, and Kubernetes resources will be added to this environment.

---

# 🧠 What you Learned

After completing this lab you understand:

- How Azure resources are organized
- The difference between a Tenant and a Subscription
- The purpose of a Resource Group
- Why Resource Groups are important
- How to create a Resource Group using Azure Portal
- How to create one using Azure CLI
- How to verify Azure resources
- Why cleanup is important in cloud environments

---

# 💬 Interview Practice

### What is an Azure Resource Group?

It is a logical container used to organize related Azure resources so they can be managed together.

### Is a Resource Group a physical location?

No. It is a logical management container.

### Why would a company use multiple Resource Groups?

Companies can use different Resource Groups to separate environments, applications, teams, or workloads.

For example:

```text
rg-production
rg-development
rg-testing
```

### Why should you be careful when deleting a Resource Group?

Because deleting the Resource Group can also delete the resources contained inside it.
