
# 🌐 Lab 02 — Understanding Azure Virtual Networking (VNET)

## 🎯 Goal

The goal of this lab is to understand how private networking works inside Microsoft Azure.

Instead of simply creating a Virtual Network, I wanted you to understand:

- What a Virtual Network is
- Why cloud resources need private networks
- What CIDR notation means
- Why we use subnets
- How IP address ranges are organized
- The difference between private and public IP addresses
- How to create and verify Azure networking resources

---

# 🌐 What is an Azure Virtual Network?

An Azure Virtual Network, commonly called a **VNet**, is a private network created inside Microsoft Azure.

It provides an IP address space where Azure resources can communicate.

For example:

```text
Azure Virtual Network
│
├── Application Server
├── Database
├── Virtual Machine
└── Other Azure Resources
```

A simple way to think about a VNet is:

> A VNet is similar to a private company network, except that it exists inside Azure.

---

# 🏢 Simple Analogy

Imagine a company office.

The company has one private network:

```text
Company Network
│
├── Employee Computers
├── Servers
├── Printers
└── Database
```

Azure can provide a similar concept:

```text
Azure VNet
│
├── Virtual Machines
├── Applications
├── Databases
└── Cloud Services
```

The major difference is that the infrastructure is virtual and managed in the cloud.

---

# 🔢 Understanding CIDR

My VNet uses the following address space:

```text
10.10.0.0/16
```

The `/16` represents the size of the network.

This network contains addresses within the range:

```text
10.10.0.0
to
10.10.255.255
```

This gives the VNet a large private address space that can be divided into smaller networks.

---

# 🧩 What is a Subnet?

A subnet divides a larger network into smaller sections.

For example, a company might separate systems based on their purpose:

```text
Company Network
│
├── Application Network
├── Database Network
└── Management Network
```

The same idea can be applied in Azure.

---

# 🏗️ My Azure Network

For this lab I created:

```text
vnet-jeff-lab
10.10.0.0/16
│
├── snet-app
│   10.10.1.0/24
│
└── snet-data
    10.10.2.0/24
```

The VNet provides the complete private address space.

The subnets divide that address space into smaller networks for different types of resources.

---

# 📦 Understanding /24 Networks

The subnet:

```text
10.10.1.0/24
```

contains 256 IPv4 addresses in total.

Its range is:

```text
10.10.1.0
to
10.10.1.255
```

Azure reserves five addresses in every subnet, so not all 256 addresses can be assigned to resources.

---

# 🤔 Why Use Multiple Subnets?

Subnets help organize and isolate different types of workloads.

For example:

```text
Internet
    │
    ▼
Application Tier
10.10.1.0/24
    │
    ▼
Database Tier
10.10.2.0/24
```

Later, security rules can control which systems are allowed to communicate between these networks.

This can help prevent sensitive systems, such as databases, from being directly exposed.

---

# 🔒 Private IP vs Public IP

A private IP address is used for communication inside private networks.

Example:

```text
10.10.1.4
```

A public IP address can be reachable from the Internet when the necessary networking and security configuration allows it.

A simple analogy is:

```text
Private IP = internal office extension

Public IP = public company phone number
```

---

# 🧪 What I Built

Resource Group:

```text
rg-jeffry-cloud-lab
```

Virtual Network:

```text
vnet-jeff-lab
```

Address space:

```text
10.10.0.0/16
```

Application subnet:

```text
snet-app
10.10.1.0/24
```

Data subnet:

```text
snet-data
10.10.2.0/24
```

---

# 💻 Azure CLI Verification

## List Virtual Networks

```bash
az network vnet list --output table
```

This command asks Azure CLI to display the Virtual Networks available in the current subscription.

---

## View the VNet

```bash
az network vnet show \
  --resource-group rg-jeffry-cloud-lab \
  --name vnet-jeff-lab \
  --output table
```

This command displays information about my specific Virtual Network.

---

## List the Subnets

```bash
az network vnet subnet list \
  --resource-group rg-jeffry-cloud-lab \
  --vnet-name vnet-jeff-lab \
  --output table
```

This verifies that the two expected subnets exist:

```text
snet-app
snet-data
```

---

# ✅ Validation

I verified the networking configuration using both:

- Azure Portal
- Azure CLI

Expected architecture:

```text
10.10.0.0/16
│
├── 10.10.1.0/24
└── 10.10.2.0/24
```

An important lesson from this lab is:

> A subnet address range must belong to the address space assigned to the VNet.

For example:

```text
10.10.1.0/24
```

belongs to:

```text
10.10.0.0/16
```

but:

```text
192.168.50.0/24
```

does not.

---

# 🔎 Troubleshooting Notes

## Problem: The subnet address range cannot be created

One possible reason is that the subnet is outside the VNet address space.

For example, if the VNet is:

```text
10.10.0.0/16
```

then:

```text
10.10.1.0/24
```

is valid.

However:

```text
192.168.1.0/24
```

would not be part of that VNet address space.

---

## Problem: Two subnets overlap

Subnet address ranges inside the same VNet cannot overlap.

For example:

```text
snet-a
10.10.1.0/24
```

and another subnet using the same range:

```text
snet-b
10.10.1.0/24
```

would conflict.

Planning IP address ranges before deploying infrastructure is therefore important.

---

# 🧠 What we've Learned

After completing this lab you can understand:

- What an Azure VNet is
- Why VNets provide private network space
- The purpose of CIDR notation
- What `/16` and `/24` represent
- Why networks are divided into subnets
- The difference between private and public IP addresses
- Why subnet ranges must belong to the VNet address space
- How to inspect VNets using Azure CLI
- How to inspect Azure subnets using Azure CLI
- Why IP planning matters before deploying cloud infrastructure

---

# 💬 Interview Practice

### What is an Azure VNet?

An Azure Virtual Network is a private network in Azure that allows cloud resources to communicate using private IP addressing.

### What is a subnet?

A subnet is a smaller network created inside a VNet address space. It can be used to separate resources based on their role or security requirements.

### Why would you create multiple subnets?

Multiple subnets can separate different workloads such as application servers, databases, and management resources.

### Can a subnet use an IP range outside its VNet address space?

No. A subnet must use an address range contained within the VNet address space.

### What is the difference between a private and public IP?

A private IP is primarily used for communication inside private networks. A public IP can provide Internet-facing connectivity when appropriate routing and security rules allow it.
