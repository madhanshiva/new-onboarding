# AI-102: Azure AI Engineer Associate Workshop

Welcome to your AI-102: Azure AI Engineer Associate workshop! We’re excited to guide you through hands-on learning with Azure AI services using Azure AI Foundry, the Azure portal, and tools like Document Intelligence, Custom Vision, the Language Service, etc., to create, deploy, and test intelligent solutions.

# Lab 2a: Create a generative AI chat app

### Overall Estimated Timing: 60 Minutes

## Overview

In this lab, you will build a generative AI chat application using Azure AI Foundry and a deployed GPT-4.1 model. You’ll begin by deploying the model in a new AI project, then connect it to a Python client application through Azure Cloud Shell. Finally, you’ll modify and run the chat app to interact with the model, maintaining conversation history and testing real-time responses.

## Objectives

By the end of this lab, you will be able to:

1. **Create and deploy a project in Azure AI Foundry**: Set up a new project, deploy models such as gpt-4.1 for use in a chat application.

2. **Create a client application:** Clone and configure a Python-based chat app to connect with your deployed model.

3. **Test the chat application with your model:** Modify the client application code to interact with the GPT-4.1 model, maintain conversation history, and test real-time responses.

## Pre-requisites

* Basic knowledge of the Azure portal.
* Familiarity with core AI concepts such as generative AI and language models.
* Basic knowledge of Python programming.

## Architecture

The lab architecture demonstrates how Azure AI Foundry supports generative AI model exploration, deployment, and testing:

1. **Azure AI Foundry Resource**: Provisioned in Azure, this resource hosts the deployed gpt-4.1 model and provides connectivity through project endpoints and keys.

2. **Azure AI Foundry Project**: A workspace where models are deployed and managed, project settings are configured, and endpoints and authorization keys are accessed for application integration.

3. **Azure Cloud Shell:** A browser-based command-line tool for cloning code, setting up environment variables, and running the Python chat app with the deployed model.


## Architecture Diagram

![](../Images/AI-102-arch-lab2a.png)

## Explanation of Components

1. **Azure AI Foundry Resource**: A cloud-based resource created in the Azure portal that connects to Azure AI services and hosts deployed models such as gpt-4.1, providing the foundation for AI project development.

2. **Azure AI Foundry Project**: A workspace within the Foundry resource where you deploy and manage models, configure project-level settings, and access endpoints and authorization keys for integrating AI capabilities into applications.

3. **Azure Cloud Shell:** A command-line environment used to clone the sample code, configure environment variables, and run the Python chat application against the deployed model.

4. **Client Application (Python Chat App):** A sample Python-based application configured with the project endpoint and model deployment to send user prompts and receive model responses.

# Getting Started with lab

Welcome to your AI-102: Azure AI Engineer Associate workshop! We’ve prepared an interactive environment for you to explore generative AI concepts and work with Microsoft Azure services like Azure AI Foundry, Document Intelligence, Custom Vision, Language Service etc. Let’s get started and make the most of this hands-on experience.

## Accessing Your Lab Environment
 
Once you're ready to dive in, your virtual machine and **Guide** will be right at your fingertips within your web browser.
 
![Access Your VM and Lab Guide](../Images/AI-102-2a-1.png)

### Virtual Machine & Lab Guide
 
Your virtual machine is your workhorse throughout the workshop. The lab guide is your roadmap to success.

## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.
 
![Explore Lab Resources](../Images/envtab.png)

## Lab Guide Zoom In/Zoom Out
 
To adjust the zoom level for the environment page, click the **A↕: 100%** icon located next to the timer in the lab environment.

![](../Images/AI-102-2a-2.png)

## Utilizing the Split Window Feature
 
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the Top right corner.
 
![Use the Split Window Feature](../Images/splitwindow.png)

## Managing Your Virtual Machine
 
Feel free to **Start, Restart, or Stop (2)** your virtual machine as needed from the **Resources (1)** tab. Your experience is in your hands!
 
![Manage Your Virtual Machine](../Images/resourcetab.png)

## Lab Progress

You can use the **Progress** tab to track your progress while working on the lab. A score will be provided after successful validation.

![](../Images/progresstab.png)

## Let's Get Started with Azure Portal
 
1. On your virtual machine, click on the Azure Portal icon as shown below:
 
   ![Launch Azure Portal](../Images/azureportalicon.png)

1. In sign-in window, kindly sign in using the provided Azure credentials

    - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

        ![](../Images/AI-l16-0.png)

    - **Password:** <inject key="AzureAdUserPassword"></inject>

        ![](../Images/AIl16-1.png)

1. If prompted to **Stay signed in?**, you can click **No**.

    ![](../Images/AIl16-2.png)

1. If a **Welcome to Microsoft Azure** pop-up window appears, simply click **Cancel** to skip the tour.

    ![](../Images/AIl16-3.png)


## Support Contact
 
The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels explicitly tailored for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.
 
Learner Support Contacts:
 
- Email Support: cloudlabs-support@spektrasystems.com
- Live Chat Support: https://cloudlabs.ai/labs-support

Click on **Next** from the lower right corner to move on to the next page.

   ![Start Your Azure Journey](../Images/nextpage.png)

## Happy Learning !!

