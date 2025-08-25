# Lab 34: Analyze forms with prebuilt Azure AI Document Intelligence models

### Estimated Duration : 30 Minutes
## Overview

In this exercise, you'll set up an Azure AI Foundry project with all the necessary resources for document analysis. You'll use both the Azure AI Foundry portal and the Python SDK to submit forms to that resource for analysis.

While this exercise is based on Python, you can develop similar applications using multiple language-specific SDKs; including:

- [Azure AI Document Intelligence client library for Python](https://pypi.org/project/azure-ai-formrecognizer/)
- [Azure AI Document Intelligence client library for Microsoft .NET](https://www.nuget.org/packages/Azure.AI.FormRecognizer)
- [Azure AI Document Intelligence client library for JavaScript](https://www.npmjs.com/package/@azure/ai-form-recognizer)

## Lab Objectives

In this lab, you'll perform the following tasks:

- **Task 1:** Create an Azure AI Foundry project

- **Task 2:**  Use the Read model
  
- **Task 3:** Prepare to develop an app in Cloud Shell

- **Task 4:** Add code to use the Azure Document Intelligence service

## Task 1: Create an Azure AI Foundry project

Let's start by creating an Azure AI Foundry project.

1.  Open a new tab in the browser, right-click on the following link [Azure AI Foundry portal](https://ai.azure.com), then **Copy link** and paste it in a browser tab to log in to **Azure AI Foundry portal**.

1. Click on **Sign in**.

   ![](../Images/AI-l31-1.png) 

1. If prompted to sign in, enter your credentials:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

   - **Password:** <inject key="AzureAdUserPassword"></inject>

1. If prompted to **Stay signed in**, you can click **No**.

1. Close any tips or quick start panes that are opened the first time you sign in, and if necessary use the Azure AI Foundry logo at the top left to navigate to the home page, which looks similar to the following image (close the Help pane if it's open)

1. In the browser, navigate to `https://ai.azure.com/managementCenter/allResources` and select **Create new**. 

    ![](../Images/AI-l34-1.png) 

1. In the Create a project wizard, select AI hub resource.

    ![](../Images/AI-l34-2.png) 

1. Enter the project name as **Myproject<inject key="DeploymentID" enableCopy="false"/> (1)**, then select **Rename hub (2)**. Then rename the hub as  **Myhub<inject key="DeploymentID" enableCopy="false"/> (3)** and then **Next (4)**.

    ![](../Images/AI-l34-3.png) 

1. Expand **Advanced options (1)**, and specify the following settings for your project and leave the rest as their defaults:

    - **Subscription**: **Default subscription (2)**
    - **Resource group**: **AI-102-RG34 (3)**
    - Region: Select **<inject key="Region" enableCopy="false" /> (4)**
    - Select **Create (5)**

      ![](../Images/AI-l34-4.png) 

    > **Note**: If you're working in an Azure subscription in which policies are used to restrict allowable resource names, you may need to use the link at the bottom of the **Create a new project** dialog box to create the hub using the Azure portal.

    > **Tip**: If the **Create** button is still disabled, be sure to rename your hub to a unique alphanumeric value.

1. Wait for your project to be created. This may take approximately **5 minutes**.

## Task 2: Use the Read model

Let's start by using the **Azure AI Foundry** portal and the Read model to analyze a document with multiple languages:

1. In the navigation panel on the left, select **AI Services (1)**, on **Azure AI Services** page, select the **Vision + Document (2)** tile.

    ![](../Images/AI-l34-5.png) 

1. On the **Vision + Document** page, verify the **Document (1)** tab is selected, then scroll down and select the **OCR/Read (2)** tile under **General document analysis models**.  

    ![](../Images/AI-l34-6.png)

    ![](../Images/AI-l34-7.png)

1. In the list of documents on the left, select **read-german.pdf (1)** and on the top toolbar, click **Analyze options (2)**.

    ![](../Images/AI-l34-8.png)

    >**Note:** In the **Read** page, the Azure AI Services resource created with your project should already be connected.

1. In the **Analyze options** pane, enable the **Language (1)** check-box under **Optional detection** and then select **Save (2)**.  

    ![](../Images/AI-l34-9.png)

1. At the top-left, select **Run Analysis**.

    ![](../Images/AI-l34-10.png)

1. When the analysis is complete, the text extracted from the image is shown on the right in the **Content** tab. Review this text and compare it to the text in the original image for accuracy.

    ![](../Images/AI-l34-11.png)
  
1. Select the **Result** tab. This tab displays the extracted JSON code. 

    ![](../Images/AI-l34-12.png)

## Task 3: Prepare to develop an app in Cloud Shell

Now let's explore the app that uses the Azure Document Intelligence service SDK. You'll develop your app using Cloud Shell. The code files for your app have been provided in a GitHub repo.

This is the invoice that your code will analyze.

![Screenshot showing a sample invoice document.](./media/sample-invoice.png)

1. In the Azure AI Foundry portal, select **Overview (1)** for your project.

1. In the **Endpoints and keys** section, select the **Azure AI Services (2)** tab, then copy the **Azure AI Services endpoint (3)** and **API Key (4)**.  

    ![](../Images/AI-l34-13.png)

    > **Note:** Save these values in a Notepad file. You will use them later in the exercise.

1. Open a new browser tab (keeping the Azure AI Foundry portal open in the existing tab). Then in the new tab, browse to the [Azure portal](https://portal.azure.com) at `https://portal.azure.com`; signing in with your Azure credentials if prompted.

1. On the **Azure portal** homepage, click the **\[>\_] Cloud Shell (1)** button located to the right of the **Copilot** tab at the top. This opens a new Cloud Shell session. In the **Welcome to Azure Cloud Shell** window, choose **PowerShell (2)**.

    ![](../Images/AI-l16-69.png)

    ![](../Images/AI-l16-70.png)

    > **Note**: If you have previously created a cloud shell that uses a **Bash** environment, switch it to **PowerShell**.

1. In the **Getting started** window, ensure **No storage account required (1)** is selected. From the **Subscription** drop-down, choose **Default subscription (2)**, then click **Apply (3)**.

    ![](../Images/AI-l16-71.png)

1. In the Cloud Shell toolbar, open the **Settings (1)** menu and choose **Go to Classic version (2)** from the drop-down.

    ![](../Images/AI-l16-72.png)

    >**Note:** **<font color="black">Ensure you've switched to the classic version of the cloud shell before continuing.</font>**

1. In the PowerShell pane, enter the following commands to clone the GitHub repo for this exercise:

    ```
   rm -r mslearn-ai-info -f
   git clone https://github.com/microsoftlearning/mslearn-ai-information-extraction mslearn-ai-info
    ```

    ![](../Images/AI-l34-14.png)

    > **Tip**: As you paste commands into the cloudshell, the ouput may take up a large amount of the screen buffer. You can clear the screen by entering the `cls` command to make it easier to focus on each task.

    ***Now follow the steps for your chosen programming language.***

1. After the repo has been cloned, navigate to the folder containing the code files:

    ```
   cd mslearn-ai-info/Labfiles/prebuilt-doc-intelligence/Python
    ```

    ![](../Images/AI-l34-15.png)

1. In the cloud shell command line pane, enter the following command to install the libraries you'll use:

    ```
   python -m venv labenv
   ./labenv/bin/Activate.ps1
   pip install -r requirements.txt azure-ai-formrecognizer==3.3.3
    ```

1. Enter the following command to edit the configuration file that has been provided. The file is opened in a code editor.

    ```
   code .env
    ```

    ![](../Images/AI-l34-16.png)

1. In the code file, replace the **YOUR_ENDPOINT** and **YOUR_KEY** placeholders with your Azure AI services endpoint and its API key (copied from the Azure AI Foundry portal).

    ![](../Images/AI-l34-17.png)

1. After you've replaced the placeholders, within the code editor, use the **CTRL+S** command to save your changes and then use the **CTRL+Q** command to close the code editor while keeping the cloud shell command line open.

## Task 4: Add code to use the Azure Document Intelligence service

Now you're ready to use the SDK to evaluate the pdf file.

1. Enter the following command to edit the app file that has been provided. The file is opened in a code editor.

    ```
   code document-analysis.py
    ```

     ![](../Images/AI-l34-18.png)

1. In the code file, find the comment **Import the required libraries** and add the following code:

    ```python
   # Add references
   from azure.core.credentials import AzureKeyCredential
   from azure.ai.formrecognizer import DocumentAnalysisClient
    ```

    ![](../Images/AI-l34-19.png)

1. Find the comment **Create the client** and add the following code (being careful to maintain the correct indentation level):

    ```python
   # Create the client
   document_analysis_client = DocumentAnalysisClient(
        endpoint=endpoint, credential=AzureKeyCredential(key)
   )
    ```

    ![](../Images/AI-l34-20.png)

1. Find the comment **Analyze the invoice** and add the following code:

    ```python
   # Analyse the invoice
   poller = document_analysis_client.begin_analyze_document_from_url(
        fileModelId, fileUri, locale=fileLocale
   )
    ```

    ![](../Images/AI-l34-21.png)

1. Find the comment **Display invoice information to the user**and add the following code:

    ```python
   # Display invoice information to the user
   receipts = poller.result()
    
   for idx, receipt in enumerate(receipts.documents):
    
        vendor_name = receipt.fields.get("VendorName")
        if vendor_name:
            print(f"\nVendor Name: {vendor_name.value}, with confidence {vendor_name.confidence}.")

        customer_name = receipt.fields.get("CustomerName")
        if customer_name:
            print(f"Customer Name: '{customer_name.value}, with confidence {customer_name.confidence}.")


        invoice_total = receipt.fields.get("InvoiceTotal")
        if invoice_total:
            print(f"Invoice Total: '{invoice_total.value.symbol}{invoice_total.value.amount}, with confidence {invoice_total.confidence}.")
    ```

    ![](../Images/AI-l34-22.png)

1. In the code editor, use the **CTRL+S** command or **Right-click > Save** to save your changes. Keep the code editor open in case you need to fix any errors in the code, but resize the panes so you can see the command line pane clearly.

1. In the command line pane, enter the following command to run the application.

    ```
    python document-analysis.py
    ```

    ![](../Images/AI-l34-23.png)

1. The program displays the **vendor name, customer name, and invoice total** with confidence levels. Compare the values it reports with the sample invoice you opened at the start of this section.

    ![](../Images/AI-l34-24.png)

