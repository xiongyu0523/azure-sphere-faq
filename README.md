# Azure Sphere FAQ

## Table of Contents
- [Azure Sphere FAQ](#azure-sphere-faq)
  - [Table of Contents](#table-of-contents)
  - [Note](#note)
  - [Product](#product)
    - [Can Azure Sphere OS run on any MCU?](#can-azure-sphere-os-run-on-any-mcu)
    - [What are the differences between Azure Sphere OS and Azure RTOS](#what-are-the-differences-between-azure-sphere-os-and-azure-rtos)
    - [Does Azure Sphere only work with Azure IoT?](#does-azure-sphere-only-work-with-azure-iot)
    - [Is Azure Sphere OS open source?](#is-azure-sphere-os-open-source)
  - [Account and administration](#account-and-administration)
    - [What is the relationship between my Azure account and Azure Sphere security service account?](#what-is-the-relationship-between-my-azure-account-and-azure-sphere-security-service-account)
    - [How can I manage a device that is claimed into someone else's tenant.](#how-can-i-manage-a-device-that-is-claimed-into-someone-elses-tenant)
  - [High-level core programming on Cortex-A7](#high-level-core-programming-on-cortex-a7)
  - [Is multiple process support?](#is-multiple-process-support)
  - [Is multiple thread support?](#is-multiple-thread-support)
  - [What does API set and Beta API mean](#what-does-api-set-and-beta-api-mean)
  - [Real-time core programming on Cortex-M4](#real-time-core-programming-on-cortex-m4)
    - [Can Cortex-M4F core start first?](#can-cortex-m4f-core-start-first)
    - [Can I deploy RTApp to a specific core?](#can-i-deploy-rtapp-to-a-specific-core)
  - [Networking](#networking)
    - [Can Azure Sphere MT3620 chip support ethernet?](#can-azure-sphere-mt3620-chip-support-ethernet)
    - [Can I set the ethernet MAC address?](#can-i-set-the-ethernet-mac-address)
    - [Can Azure Sphere support cellular network (e.g.LTE or NB-IoT)](#can-azure-sphere-support-cellular-network-eglte-or-nb-iot)
    - [What is the maximum networking throught on Azure Sphere MT3620](#what-is-the-maximum-networking-throught-on-azure-sphere-mt3620)
  - [Memory](#memory)
    - [How many memory do I have for application on Azure Sphere](#how-many-memory-do-i-have-for-application-on-azure-sphere)
    - [What is the expected performance difference for running code on CM4 TCM](#what-is-the-expected-performance-difference-for-running-code-on-cm4-tcm)
    - [Can I store file on Azure Sphere?](#can-i-store-file-on-azure-sphere)
  - [Digital and Analog I/O](#digital-and-analog-io)
    - [Does Azure Sphere support external interrupt?](#does-azure-sphere-support-external-interrupt)
    - [Can I use CA7 Debug UART (Pin 94-97 of MT3620) in my appliation to print debug messages?](#can-i-use-ca7-debug-uart-pin-94-97-of-mt3620-in-my-appliation-to-print-debug-messages)
  - [Over-the-Air deployment](#over-the-air-deployment)
  - [Manufacturing](#manufacturing)
    - [Where can I get manufacturing package?](#where-can-i-get-manufacturing-package)
    - [Do I need update my Azure Sphere OS in factory?](#do-i-need-update-my-azure-sphere-os-in-factory)
    - [Can I program](#can-i-program)

## Note

This document is the non-official FAQ to help you to understand all aspect of Azure Sphere solution from product to development and manufacturing. 

## Product 

### Can Azure Sphere OS run on any MCU?

No, Azure Sphere OS is designed to work with Azure Sphere certified MCU. Currently the only support MCU is MT3620 from Mediatek. Later on, Microsoft is working with NXP and Qualcomm to bring new Azure Sphere MCUs to market.

### What are the differences between Azure Sphere OS and Azure RTOS

Azure Sphere OS can only run on the high-level core of a Azure Sphere certified MCU and it is primarily focusing on evolving cybersecurity challenges of the internet and easy-of-use application development. Azure Sphere OS is a linux based operating system contains highly customized kernel and several services written and maintained by Microsoft.

Azure RTOS is a re-branding of Express Logic X-Wave 

### Does Azure Sphere only work with Azure IoT?

No, Azure Sphere solution is designed to be cloud agnostic. Customer can use any cloud or on prem datacenter with Sphere. The Azure Sphere security service only provides security check and ota service to Azure Sphere MCU, it has nothing to do with customer data. 

> As of 20.08 OS update 1, Azure Sphere only support DAA client authentication using Libcurl. Such level of support make sure we can fully leverage the hardware root-of-trust to have a strong client-side authentication. 

### Is Azure Sphere OS open source?

Azure Sphere OS is made by several components and not all of them are open source. The modified Linux kernel is available on Microsofo Third party disclosures website: https://3rdpartysource.microsoft.com by seaching `azure sphere`.

## Account and administration

### What is the relationship between my Azure account and Azure Sphere security service account?

### How can I manage a device that is claimed into someone else's tenant. 

You can provide your Azure Sphere security service account to the tenant owner to reuqest they add you as a contributor role to manage devices. See below command or find more details [here].(https://docs.microsoft.com/en-us/azure-sphere/deployment/add-tenant-users)

`azsphere role add --role Contributor --user <email-address>`

## High-level core programming on Cortex-A7

## Is multiple process support?

No, Azure Sphere OS only support a single user process. 

## Is multiple thread support?

Yes, Azure Sphere OS support POSIX pthread. Remember each thread creates addtional memory overhead (~8KB), and we have max. 256KB availale on A7

## What does API set and Beta API mean

Beta API is not 




## Real-time core programming on Cortex-M4

### Can Cortex-M4F core start first?

No, Cortex-M4F is started by Cortex-A7. 

### Can I deploy RTApp to a specific core?

No, tool always deploy to the first available core.

## Networking

### Can Azure Sphere MT3620 chip support ethernet?

Yes. Though MT3620 does not have integrated ethernet MAC controller, Microsoft designed an dual chip solution using a SPI connected ethernet controller:Microchip [ENC28J60](https://www.microchip.com/wwwproducts/en/en022889). A board configuration package is provided along with application. Refer to [Connect to Ethernet](https://docs.microsoft.com/en-us/azure-sphere/network/connect-ethernet) section in official document for more details. 

If you're looking for dual ethernet, Wiznet has developed a Azure Sphere guardian module [ASG200](https://www.wiznet.io/azure-sphere/) to support this scenario. The devices from downstream private network can be connnected to guardian module using Wiznet W5500 ethernet controller with integrated TCP/IP stack. 

### Can I set the ethernet MAC address?

As of 20.08 OS update 1, Azure Sphere OS will generate a random number during recover process in presistent memory for ethernet MAC address purpose.

### Can Azure Sphere support cellular network (e.g.LTE or NB-IoT)

### What is the maximum networking throught on Azure Sphere MT3620

The MT3620 support 802.11n 54Mbps, the actual UDP/TCP networking performance can be measured using iperf3 tool found in manufacturing package.

## Memory

### How many memory do I have for application on Azure Sphere

### What is the expected performance difference for running code on CM4 TCM 

### Can I store file on Azure Sphere?

Yes but restricted. There're two types of file supported on Azure Sphere OS and you need Azure Sphere API to access them.

- Developer can attach unlimited number of **ReadOnly** resource file together with application binary wthin an imagepackage, as long as the total size of flash memory will be less than 1MB. See [ReadOnly]() document for more details

- Developer can open a single mutual stroage file. The max. size of this file can be specified in app_manifest.json file up to 64KB. See [Mutual Storage]() document for more details

## Digital and Analog I/O

### Does Azure Sphere support external interrupt?

No, as of 20.08 OS update 1, Azure Sphere MT3620 does not support external pin interrupt on either A7 or M4.

### Can I use CA7 Debug UART (Pin 94-97 of MT3620) in my appliation to print debug messages?

No, CA7 Debug UART is reserved by internal use

## Over-the-Air deployment



## Manufacturing

### Where can I get manufacturing package?

Contact your technical specialist representive.

### Do I need update my Azure Sphere OS in factory?

### Can I program 
