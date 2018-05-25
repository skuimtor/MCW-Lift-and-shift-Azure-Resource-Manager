![](images/HeaderPic.png "Microsoft Cloud Workshops")

# Azure Resource Manager

## Hands-on lab unguided

## March 2018

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

# Contents

<!-- TOC -->

- [Azure Resource Manager](#azure-resource-manager)
    - [Hands-on lab unguided](#hands-on-lab-unguided)
    - [March 2018](#march-2018)
- [Contents](#contents)
- [Lift and shift hands-on lab unguided](#lift-and-shift-hands-on-lab-unguided)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Before the hands-on lab (HOL)](#before-the-hands-on-lab-hol)
        - [Task 1: Create a virtual machine for your lab environment](#task-1--create-a-virtual-machine-for-your-lab-environment)
        - [Task 2: Connect to the VM and download the student files](#task-2--connect-to-the-vm-and-download-the-student-files)
        - [Task 3: Validate connectivity to Azure](#task-3--validate-connectivity-to-azure)
    - [Exercise 1: Configure Automation account](#exercise-1--configure-automation-account)
        - [Task 1: Create Azure Automation account](#task-1--create-azure-automation-account)
    - [Exercise 2: Define the network foundation](#exercise-2--define-the-network-foundation)
        - [Task 1: Define the network foundation](#task-1--define-the-network-foundation)
    - [Exercise 3: Extend with Compute](#exercise-3--extend-with-compute)
        - [Task 1: Add storage and set to use Premium](#task-1--add-storage-and-set-to-use-premium)
        - [Task 2: Add a virtual machine for the web server](#task-2--add-a-virtual-machine-for-the-web-server)
        - [Task 3: Add a virtual machine for the SQL Server](#task-3--add-a-virtual-machine-for-the-sql-server)
        - [Task 4: Deploy completed solution to Azure](#task-4--deploy-completed-solution-to-azure)
    - [Exercise 4: Lock down the environment](#exercise-4--lock-down-the-environment)
        - [Task 1: Restrict traffic to the web server](#task-1--restrict-traffic-to-the-web-server)
        - [Task 2: Update the network security group to allow Windows Remote Desktop](#task-2--update-the-network-security-group-to-allow-windows-remote-desktop)
    - [Exercise 5: Scale out the deployment](#exercise-5--scale-out-the-deployment)
        - [Task 1: Parameterize the size of the environment and add load balancing](#task-1--parameterize-the-size-of-the-environment-and-add-load-balancing)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete the resource groups created](#task-1--delete-the-resource-groups-created)

<!-- /TOC -->

# Lift and shift hands-on lab unguided

## Abstract and learning objectives 

In this lab, attendees will learn how to author an Azure Resource
Manager (ARM) template that can be used to deploy infrastructure such as
virtual machine, storage, and networking. This lab will also teach the
attendees how to deploy virtual machines that are automatically
configured by the Azure Automation Desired State Configuration (DSC)
service.

-   How to author and deploy an ARM template

-   How to perform configuration management with Azure Automation DSC

## Overview

Contoso has asked you to define an Azure Resource Manager (ARM) template
that can deploy their application CloudShop and its associated database
using Azure Virtual Machines.

## Solution architecture

![This image is a representation of the Solution Architecture.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image2.png "Solution Architecture")

## Requirements

1.  Azure Subscription

2.  Understanding of Azure Infrastructure as a Service components

3.  Familiarity with JavaScript Object Notation (JSON)

4.  Familiarity with PowerShell

## Before the hands-on lab (HOL)

Duration: 15 minutes

Prior to attending the lab, follow the instructions below to create a
lab environment using an Azure Virtual Machine and download the needed
files for the lab exercise.

### Task 1: Create a virtual machine for your lab environment 

1.  Launch a browser using incognite or in-private mode, and navigate to
    <https://portal.azure.com>. Once prompted, login with your Microsoft
    Azure credentials. If prompted, choose whether your account is an
    organization account or just a Microsoft Account.

2.  Click on +NEW, and in the search box, type in Visual Studio
    Community 2017 on Windows Server 2016 (x64), and press enter. Click
    the Visual Studio Community 2017 image running on Windows Server
    2016 and with the latest update.

3.  In the returned search results, click the image name.

    ![In the Everything blade, Visual Studio Community 2017 is typed in the Search field. Under Name, Visual Studio Community on Windows Server 2016 is circled.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image3.png "Everything blade")

4.  In the Marketplace solution blade, click **Create**.

5.  Set the following configuration on the Basics tab, and click **OK**.

    -   Name: **LABVM**

    -   VM disk type: **SSD**

    -   User name: **demouser**

    -   Password: **demo\@pass123**

    -   Subscription: **If you have multiple subscriptions choose the subscription to execute your labs in.**

    -   Resource Group: **OPSLABRG**

    -   Location: **Choose the closest Azure region to you.**

6.  Choose the **DS1\_V2 Standard** instance size on the Size blade.

7.  Accept the remaining default values on the Settings blade, and click
    **OK**. On the Summary page, click **OK**. The deployment should
    begin provisioning. It may take more than 10 minutes for the virtual
    machine to complete provisioning.

    ![Screenshot of the Deploying Visual Studio Community 2017 icon.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image4.png "Deploying Visual Studio Community 2017 icon")

### Task 2: Connect to the VM and download the student files

1.  Move back to the Portal page on your local machine, and wait for **LABVM** to show the Status of **Running**. Click **Connect** to establish a new Remote Desktop Session.

    ![The Connect button is circled on the Portal page.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image5.png "Connect button")

2.  Depending on your remote desktop protocol client and browser
    configuration, you will either be prompted to open an RDP file, or
    you will need to download it and then open it separately to connect.
    You may also be required to click, **Use a different account**.

    ![In the Windows Security dialog box, demouser is typed in the credentials field, and the password is hidden. At the bottom, the link to Use a different account is circled.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image6.png "Windows Security dialog box")

3.  Login with the credentials specified during creation:

    a.  User: **demouser**

    b.  Password: **demo\@pass123**

4.  You will be presented with a Remote Desktop Connection warning because of a certificate trust issue. Click, **Don't ask me again for connections to this computer** followed by clicking **Yes** to continue with the connection.

    ![The Remote Desktop Connection warning page explains that the identity of the remote computer cannot be identified, and asks if you still want to connect. The checkbox is selected and circled to not ask again for connections to this computer. At the bottom, the Yes button is circled.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image7.png "Remote Desktop Connection warning page")

5.  When logging on for the first time, you will see a prompt on the right asking about network discovery. Click **No**.

    ![The Networks prompt asking if you want your PC to be discoverable, and the No button is circled.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image8.png "Networks prompt")

6.  Notice the Server Manager opens by default. On the left, click **Local Server**.

    ![Local Server is circled in the left Server Manager menu.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image9.png "Local Server")

7.  On the right side of the pane, click **On** by **IE Enhanced Security Configuration**.

    ![In Local Server information, IE Enhanced Security Configuration is circled, and is set to On.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image10.png "Local Server information")

8.  Change to **Off** for Administrators, and click **OK**.

    ![On the Internet Explorer Enhanced Security Configuration page, under Administrators, the radio button is selected for Off. At the bottom, the OK button is circled.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image11.png "Internet Explorer Enhanced Security Configuration page")

9.  In the lower left corner, click Internet Explorer to open it. On
    first use, you will be prompted about security settings. Accept the
    defaults by clicking **OK**.

    ![In the Internet Explorer 11 Setup window, the checkbox is selected for \"Use recommended security, privacy, and compatibility settings. At the bottom, the OK button is circled.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image12.png "Internet Explorer 11 Setup window")

10. If prompted, click **Don't show this again** regarding protected mode.

11. To download the exercise files for the lab, paste this URL into the browser.

    <https://cloudworkshop.blob.core.windows.net/arm-hackathon/ARM_Hackathon_Guide_Student_Files.zip>

12. You will be prompted about what you want to do with the file. Select **Save**.

    ![In the Internet Explorer Save, window, a prompt asks what you want to do with the file, and the Save button is circled.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image13.png "Internet Explorer Save window")

13. Download progress is shown at the bottom of the browser window. When the download is complete, click **Open folder**.

    ![The Open Folder button is circled on the Download progress bar.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image14.png "Download progress bar")

14. The **Downloads** folder will open. ***Right-click*** the zip file, and click **Extract All**. In the **Extract Compressed (Zipped) Folders** window, enter **C:\\Hackathon** in the **Files will be extracted to this folder** dialog. Click the **Extract** button.

### Task 3: Validate connectivity to Azure

1.  Within the virtual machine, launch **Visual Studio 2017**, and validate you can login with your Microsoft Account when prompted.

    ![The Visual Studio sign-in page displays.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image15.png "Visual Studio sign-in page")

2.  Validate connectivity to your Azure subscription. Launch **Visual Studio**, open **Server Explorer** from the View menu, and ensure you can connect to your Azure subscription.

    ![In the Visual Studio Server Explorer window, the sub-menu displays for the Azure subscription, confirming that a connection to the Azure subscription can be made.](images/Hands-onlabstep-by-step-AzureResourceManagerimages/media/image16.png "Visual Studio Server Explorer")

You should follow all steps provided *before* attending the hands-on lab.

## Exercise 1: Configure Automation account

Duration: 15 minutes

Contoso has asked you to create an ARM template that will configure the
network for a proof of concept deployment of their new application
CloudShop. Before creating the ARM Template, you must first create and
configure an Azure Automation account using the Azure portal.

### Task 1: Create Azure Automation account

Tasks to complete

-   Create Automation account in Azure portal.

-   Upload the Web and SQL .ps1 files found in the C:\\Hackathon
    directory (downloaded student files from Exercise 0 -- Task 2).

-   Compile the files so they are available to virtual machines that
    check-in.

Exit criteria

-   You log into the portal and verify creation of the Azure Automation
    account, taking note of the Registration URL, Registration Keys, and
    the names of the nodes you compiled.

## Exercise 2: Define the network foundation

Duration: 15 minutes

Contoso has asked you to create an ARM template that will configure the
network for a proof of concept deployment of their new application
CloudShop.

### Task 1: Define the network foundation

Tasks to complete

-   Create a new ARM template solution using Visual Studio and name the
    Project **ARMHackathon** in the **C:\\Hackathon** directory.

-   Using Visual Studio, create a Virtual Network in the template named
    **hackathonVnet**.

-   Rename **Subnet1** to **FrontEndNet**

-   Rename **Subnet2** to **DatabaseNet**

-   Using Visual Studio, deploy the Virtual Network to Azure in a
    Resource Group called **ARMHackathon**.

Exit criteria

-   A Virtual Network deployed to your Azure subscription containing the
    following virtual network characteristics:

    -   Resource Group: **ARMHackathon**

    -   Name: **hackathonVNET**

    -   Address space: **10.0.0.0/16**

    -   Subnet: FrontEndNet -- **10.0.0.0/24**

    -   Subnet: DatabaseNet -- **10.0.1.0/24**

## Exercise 3: Extend with Compute

Duration: 60 minutes

The next task is to update the template by adding a storage account, a
Public IP, and virtual machines for the web server(s) and a SQL server.
The servers will be automatically configured using a PowerShell DSC
script supplied to you by the infrastructure team.

### Task 1: Add storage and set to use Premium

Tasks to complete

-   Add a new storage account to the template named **hackstorage**. All
    virtual machines in the template will use this storage account.

-   Change the default storage account type to use Premium Storage.

Exit criteria

-   A new storage account in the template.

-   The new storage account should default to using Premium Storage in
    the template.

### Task 2: Add a virtual machine for the web server

Tasks to complete

-   Add the following resources to the template:

    -   A Windows VM for the web front-end named **hackathonVM**.

    -   Change the default Windows OS Version to be **2016-Datacenter**,
        and make sure that is the only OS Version that can be selected.

    -   Update the size of the VM to be a Standard DS1 version 2 so that
        it can use Premium Storage.

    -   Add a Public IP named **hackathonPublicIP** linked to the NIC of
        the web VM.

    -   Azure DSC Extension

        -   This DSC Extension is named **hackathonDSC** for the web
            servers that use the CloudShopWeb.WebServer configuration
            node of your Automation account.

Exit criteria

-   Windows VM that will be running Windows Server Datacenter 2016 with
    a Public IP was added to the template. This VM will check-in with
    Azure Automation DSC to receive its configuration upon provisioning
    and continue to check in to ensure its configuration is correct.

-   The template should accept parameters for the Registration URL,
    Registration Key, and Configuration Node name(s) of the Automation
    account.

### Task 3: Add a virtual machine for the SQL Server

Tasks to complete

-   Add the following resources to the template:

    -   A Windows VM for the databased backend named **hackathonSqlVM**.

    -   Update the Image Publisher to allow for SQL Server Images, and
        insure that the SQL Server version deployed will be 2016.

    -   Replace the new SQL VM's WindowsOSVersion parameter with a new
        parameter that allows the administrators to choose from the
        different versions of SQL available for 2016: Web, Standard,
        Enterprise.

    -   Add a new parameter to allow the Administrator to choose which
        size of SQL Server they want from Standard DS1 -- Standard DS5
        version 2.

    -   Add two empty 1 TB Data Disks to the SQL Server.

    -   Azure DSC Extension

        -   This DSC Extension is named **hackathonDSCSql** for the SQL
            database that configures the SQL Server and restores the
            AdventureWorks database.

        -   The scripts for the DSC Configurations are in C:\\Hackathon.

        -   Add a dependency on the DSC extension for the web tier to
            the DSC extension for the SQL server. This allows for the
            virtual machines to be provisioned in parallel but the
            customizations to be orchestrated.

Exit criteria

-   SQL Server virtual machine named **hackathonSqlVM**

    -   The variables named **hackathonSQLVMImagePublisher** and
        **hackathonSqlVMImageOffer** that contain the image
        configuration should use the following:

        -   Image Publisher: **MicrosoftSQLServer**

        -   Image Offer: **SQL2016-WS2016**

    -   The parameter **hackathonSQLVMWindowsOSVersion** should be
        deleted.

    -   The VM should accept a parameter named **hackathonSqlVMSKU**
        that allows the user to specify the SQL Server SKU using one of
        the following values. The **default** value should be **Web**.

        -   **Web, Standard, Enterprise**

    -   Should have a parameter that specifies the size of the virtual
        machine. The default value should be Standard\_DS2.

        -   Standard\_DS1\_v2, Standard\_DS2\_v2, Standard\_DS3\_v2,
            Standard\_DS4\_v2, Standard\_DS5\_v2

    -   The virtual machine should have 2 x 1 TB Data Disks attached.

    -   The PowerShell DSC Script will perform the following based on
        this config:

        -   Data Disks combined into a 2TB volume using Windows Storage
            Spaces.

        -   The SQL Server should have the default path for data, logs,
            and backup referring to the F: drive and the database should
            be restored to the F: drive.

        -   SQL Server should be in mixed authentication mode.

### Task 4: Deploy completed solution to Azure

Tasks to complete

-   Save all changes to the Visual Studio Solution named ARMHackathon.

-   Copy the contents of azuredeploy.json into
    [www.jsonlint.com](http://www.jsonlint.com), and determine if the
    json is clean.

-   Using Visual Studio perform a new deployment.

-   After the deployment is complete, check the status of the Servers in
    Azure Automation DSC. Once they show compliant, move on to the next
    task.

-   Locate the DNS name used for the web front-end VM. Paste this name
    into a new browser window.

Exit criteria

-   All the resources you added are now found in the **ARMHackathon**
    resource group.

-   The website loads, and the CloudShop application is now useable.

    ![The Cloud Shop webpage displays, with a list of products from
    which to
    choose.](C:\Users\MichaelWasham\Dropbox (Opsgility)\Opsgility Projects\active\Cloud Workshops\Lift and Shift\Markdown Convert\Hands-onlabunguided-AzureResourceManagerimages/media/image18.emf "Cloud Shop webpage")

## Exercise 4: Lock down the environment 

Duration: 15 minutes

In this portion of the hackathon, you will deploy a network security
group to restrict the network attack surface for the deployment.

### Task 1: Restrict traffic to the web server

Tasks to complete

-   Create and deploy a network security group (NSG), named
    **hackathonNetworkSecurityGroup** that allows the following traffic
    on the **FrontEndNet** subnet.

    -   Source: INTERNET, Destination port: 80

-   Update the **hackathonVnet** to show it depends on the newly created
    **hackathonNetworkSecurityGroup**.

Exit criteria

-   The NSG is now deployed to the **ARMHackathon** resource group.

-   Validate restrictions by connecting to the web server using a web
    browser. Traffic should be allowed in, and the site should load.

-   Attempt to connect using remote desktop and click **Connect** in the
    portal. This connection should fail.

-   The **hackathonVnet** **dependsOn** array should be updated to
    reference **hackathonNetworkSecurityGroup**.

### Task 2: Update the network security group to allow Windows Remote Desktop 

Tasks to complete

-   Update the network security group on the deployed resources to allow
    the remote desktop protocol (port 3389).

Exit Criteria

-   Access to the web front-end should be successful by clicking
    **Connect** in the Azure Preview Portal.

-   The website should still load when directing a web browser to the
    web frond end VM's Public IP Address.

-   In the Azure portal, there should now be 2 Inbound Security Rules:
    webrule for HTTP/80 and rdprule for TCP/3389.

## Exercise 5: Scale out the deployment

Duration: 60 minutes

In this portion of the exercise, you will use the Azure Virtual Machine
Scale Sets feature to scale out the web front-end and the storage sub
system.

Note: This is an *advanced* configuration based on the feature Azure
Scale Sets. It is recommended that unless you have configured scale sets
before that you use the Leader's guide with the hackathon answers to
complete this exercise.

### Task 1: Parameterize the size of the environment and add load balancing

Tasks to complete

-   Delete the **ARMHackathon** resource group using the Azure portal.

-   Using Visual Studio update your solution:

    -   Remove the existing virtual machine and network adapter because
        the scale set replaces them.

    -   Add a new parameter called **instanceCount** to the template.
        This variable should define how many web servers should be
        created.

    -   Add a new parameter called **newStorageAccountSuffix** with type
        string. This will be used to build the names of the storage
        accounts for the scale set VMs.

    -   In Resources add a storage accounts section that will
        concatenate the names using the **newStorageAccountSuffix**
        parameter and use the copyIndex to build 5 storage accounts in a
        loop.

    -   Add a Load Balancer that will use the Public IP address as the
        FrontEnd Pool and the VM Scale Set as the Backend Pool. It will
        also need a TCP probe on port 80 with the floating IP option set
        to False. Also, add an HTTP probe on port 80 with a request path
        of "/".

    -   Add the virtual machine scale set configuration that will use
        the storage accounts and creates several virtual machines based
        on the **instanceCount** parameter.

-   Deploy this solution to a new resource group named
    **ARMHackathonScaleSets**

Exit criteria

-   The ARMHackathon RG and its contents have been deleted.

-   A new RG named **ARMHackathonScaleSets** has been created with all
    the correct resources from your ARM template.

-   The scale set should provision the number of virtual machines based
    on the instanceCount parameter.

-   The virtual machines provisioned by the scale set should execute the
    same DSC script used in the initial web server configuration.

-   The SQL Server has executed the SQL DSC script used in the initial
    SQL server configuration.

-   Once the machines show as "Compliant" in the DSC nodes blade of the
    Azure portal, the website loads by pointing a web browser to the
    Public IP DNS name which is now associated with the Load Balancer.

## After the hands-on lab 

Duration: 10 minutes

### Task 1: Delete the resource groups created

1.  Within the Azure portal, click Resource Groups on the left
    navigation.

2.  Delete each of the resource groups created in this lab by clicking
    them followed by clicking the Delete Resource Group button. You will
    need to confirm the name of the resource group to delete.

You should follow all steps provided *after* attending the hands-on lab.
