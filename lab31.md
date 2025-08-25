# Lab 31: Generate images with AI

### Estimated Duration : 30 Minutes
## Overview

In this exercise, you use the OpenAI DALL-E generative AI model to generate images. You also use the OpenAI Python SDK to create a simple app to generate images based on your prompts.

> **Note**: This exercise is based on pre-release SDK software, which may be subject to change. Where necessary, we've used specific versions of packages; which may not reflect the latest available versions. You may experience some unexpected behavior, warnings, or errors.

While this exercise is based on the OpenAI Python SDK, you can develop AI chat applications using multiple language-specific SDKs; including:

* [OpenAI Projects for Microsoft .NET](https://www.nuget.org/packages/OpenAI)
* [OpenAI Projects for JavaScript](https://www.npmjs.com/package/openai)

## Lab Objectives

In this lab, you'll perform the following tasks:

- **Task 1:** Choose a model to start a project

- **Task 2:** Test the model in the playground
  
- **Task 3:** Test your agent

## Task 1: Choose a model to start a project

An Azure AI *project* provides a collaborative workspace for AI development. Let's start by choosing a model that we want to work with and creating a project to use it in.

> **Note**: AI Foundry projects can be based on an *Azure AI Foundry* resource, which provides access to AI models (including Azure OpenAI), Azure AI services, and other resources for developing AI agents and chat solutions. Alternatively, projects can be based on *AI hub* resources; which include connections to Azure resources for secure storage, compute, and specialized tools. Azure AI Foundry based projects are great for developers who want to manage resources for AI agent or chat app development. AI hub based projects are more suitable for enterprise development teams working on complex AI solutions.

1.  Open a new tab in the browser, right-click on the following link [Azure AI Foundry portal](https://ai.azure.com), then **Copy link** and paste it in a browser tab to log in to **Azure AI Foundry portal**.

1. Click on **Sign in**.

   ![](../Images/AI-l31-1.png) 

1. If prompted, provide the credentials below:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

   - **Password:** <inject key="AzureAdUserPassword"></inject>

1. In the home page, in the **Explore models and capabilities** section, search for the `dall-e-3` **(1)** model and select `dall-e-3` **(2)** from the results.

    ![](../Images/AI-l31-2.png) 

1. On the **dall-e-3** model details page, click **Use this model** at the top to continue.  

     ![](../Images/AI-l31-3.png) 

1. In the **Create a project to work with dall-e-3** window, enter **Myproject<inject key="DeploymentID"></inject> (1)** as the project name. Open the **Advanced options (2)** drop-down, fill in the following details, and then click **Create (6)**:

    * Subscription: **Choose Default Subscription (3)**
    * Resource group: **AI-102-RG31 (4)**
    * Region: **<inject key="Region"></inject> (5)**

      ![](../Images/AI-l31-4.png) 

      >**Note:** Some Azure AI resources are constrained by regional model quotas. In the event of a quota limit being exceeded later in the exercise, there's a possibility you may need to create another resource in a different region.

1. Depending on your model selection you might receive additional prompts during the project creation process. Agree to the terms and finalize the deployment.

1. On the **Deploy dall-e-3** page, keep the default settings, review the deployment details, and then click **Create resource and deploy**.  

    ![](../Images/AI-l31-5.png) 

1. When your project is created, your model will be displayed in the **Models + endpoints** page.

## Task 2: Test the model in the playground

Before creating a client application, let's test the DALL-E model in the playground.

1. In the left navigation pane, select **Playgrounds (1)**. Under **Images playground**, click **Try the Images playground (2)**.

    ![](../Images/AI-l31-8.png) 

1. On the **Images playground** page, ensure the **dall-e-3 (1)** deployment is selected. In the prompt box, enter a description such as `Create an image of a robot eating spaghetti (2)` and click **Generate (3)** to create the image.  

    ![](../Images/AI-l31-9.png) 

1. Review the resulting image in the playground:

    ![](../Images/AI-l31-10.png) 

1. Enter a follow-up prompt such as `Show the robot in a restaurant (1)` and click **Generate (2)**.

   ![](../Images/AI-l31-11.png)

1. Review the generated image displayed in the **Images playground**. 

    ![](../Images/AI-l31-12.png) 

1. Continue testing with new prompts to refine the image until you are happy with it. 

1. Select the **\</\> View Code** button and ensure you are on the **Entra ID authentication** tab. Then record the following information for use later in the exercise. Note the values are examples, be sure to record the information from your deployment.

    * OpenAI Endpoint: *https://dall-e-aus-resource.cognitiveservices.azure.com/*
    * OpenAI API version: *2024-04-01-preview*
    * Deployment name (model name): *dall-e-3*

      ![](../Images/AI-l31-13-key.png) 

## Task 3: Create a client application

The model seems to work in the playground. Now you can use the OpenAI SDK to use it in a client application.

### Prepare the application configuration

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

1. In the cloud shell pane, enter the following commands to clone the GitHub repo containing the code files for this exercise (type the command, or copy it to the clipboard and then right-click in the command line and paste as plain text):

    ```
    rm -r mslearn-ai-vision -f
    git clone https://github.com/MicrosoftLearning/mslearn-ai-vision
    ```

    ![](../Images/AI-l31-14.png) 

    > **Tip**: As you paste commands into the cloudshell, the ouput may take up a large amount of the screen buffer. You can clear the screen by entering the `cls` command to make it easier to focus on each task.

1. After the repo has been cloned, navigate to the folder containing the application code files:  

    ```
   cd mslearn-ai-vision/Labfiles/dalle-client/python
    ```

    ![](../Images/AI-l31-15.png) 

1. In the cloud shell command line pane, enter the following command to install the libraries you'll use:

    ```
   python -m venv labenv
   ./labenv/bin/Activate.ps1
   pip install -r requirements.txt azure-identity openai requests
    ```

1. Enter the following command to edit the configuration file that has been provided. The file is opened in a code editor.

    ```
   code .env
    ```

    ![](../Images/AI-l31-16.png) 

1. Replace the **your_endpoint**, **your_model_deployment**, and **your_api_version**  placeholders with the values you recorded from the from the **Images playground**.

    ![](../Images/AI-l31-17.png) 

1. After you've replaced the placeholders, use the **CTRL+S** command to save your changes and then use the **CTRL+Q** command to close the code editor while keeping the cloud shell command line open.

## Task 4: Write code to connect to your project and chat with your model

> **Tip**: As you add code, be sure to maintain the correct indentation.

1. Enter the following command to edit the code file that has been provided:

    ```
   code dalle-client.py
    ```

    ![](../Images/AI-l31-18.png) 

1. In the code file, note the existing statements that have been added at the top of the file to import the necessary SDK namespaces. Then, under the comment **Add references**, add the following code to reference the namespaces in the libraries you installed previously:

    ```python
   # Add references
   from dotenv import load_dotenv
   from azure.identity import DefaultAzureCredential, get_bearer_token_provider
   from openai import AzureOpenAI
   import requests
    ```

   ![](../Images/AI-l31-19.png) 

1. In the **main** function, under the comment **Get configuration settings**, note that the code loads the endpoint, API version, and model deployment name values you defined in the configuration file.

1. Under the comment **Initialize the client**, add the following code to connect to your model using the Azure credentials you are currently signed in with:

    ```python
   # Initialize the client
   token_provider = get_bearer_token_provider(
       DefaultAzureCredential(exclude_environment_credential=True,
           exclude_managed_identity_credential=True), 
       "https://cognitiveservices.azure.com/.default"
   )
    
   client = AzureOpenAI(
       api_version=api_version,
       azure_endpoint=endpoint,
       azure_ad_token_provider=token_provider
   )
    ```

    ![](../Images/AI-l31-20.png) 

1. Note that the code includes a loop to allow a user to input a prompt until they enter "quit". Then in the loop section, under the comment **Generate an image**, add the following code to submit the prompt and retrieve the URL for the generated image from your model:

    **Python**

    ```python
   # Generate an image
   result = client.images.generate(
        model=model_deployment,
        prompt=input_text,
        n=1
    )

   json_response = json.loads(result.model_dump_json())
   image_url = json_response["data"][0]["url"] 
    ```

     ![](../Images/AI-l31-21.png) 

1. Note that the code in the remainder of the **main** function passes the image URL and a filename to a provided function, which downloads the generated image and saves it as a .png file.

1. Use the **CTRL+S** command to save your changes to the code file and then use the **CTRL+Q** command to close the code editor while keeping the cloud shell command line open.

## Task 5: Run the client application

1.In the Cloud Shell command-line pane, enter the following command **(1)** to sign into Azure. Click the **link (2)** displayed in the output and Copy the **code (3)** provided to authenticate.  


    ```
   az login
    ```
     ![](../Images/AI-l31-22.png)
   
    >**Note:** **<font color="red">You must sign into Azure - even though the cloud shell session is already authenticated.</font>**

    >**Note:** In most scenarios, just using *az login* will be sufficient. However, if you have subscriptions in multiple tenants, you may need to specify the tenant by using the *--tenant* parameter. See [Sign into Azure interactively using the Azure CLI](https://learn.microsoft.com/cli/azure/authenticate-azure-cli-interactively) for details.

1. In the new browser tab, when the **Enter code to allow access window** appears, paste the copied code **(1)** and select **Next (2)**.

    ![](../Images/AI-l31-23.png)

1. On the **Pick an account** page, select the **ODL_User<inject key="DeploymentID"></inject> (1)** to sign in.  

    ![](../Images/AI-l31-24.png)

1. In the **Are you trying to sign in to Microsoft Azure CLI?** dialog box, click **Continue**.

     ![](../Images/AI-l31-25.png)

1. When the **Microsoft Azure Cross-platform Command Line Interface** window pops up, return to the browser tab with Cloud Shell open. 

    ![](../Images/AI-l31-26.png)

1. In the Cloud Shell console, press **Enter** to select the only available subscription.

    ![](../Images/AI-l31-27.png)

1. In the cloud shell command line pane, enter the following command to run the app:

    ```
   python dalle-client.py
    ```

     ![](../Images/AI-l31-28.png)

1. When prompted, enter a request for an image, such as `Create an image of a robot eating pizza`. After a moment or two, the app should confirm that the image has been saved.

     ![](../Images/AI-l31-29.png)

1. Try a few more prompts. When you're finished, enter `quit` to exit the program.

    > **Note**: In this simple app, we haven't implemented logic to retain conversation history; so the model will treat each prompt as a new request with no context of the previous prompt.

1. To download and view the images that were generated by your app, use the cloud shell **download** command - specifying the .png file that was generated:

    ```
   download ./images/image_1.png
    ```
1. The download command creates a popup link at the bottom right of your browser, which you can select to download and open the file.

    ![](../Images/AI-l31-30.png)

1. The downloaded image should display the generated result, similar to the example shown below:  

    ![](../Images/AI-l31-31.png)

## Summary

In this exercise, you used Azure AI Foundry and the Azure OpenAI SDK to create a client application uses a DALL-E model to generate images.

## Clean up

If you've finished exploring DALL-E, you should delete the resources you have created in this exercise to avoid incurring unnecessary Azure costs.

1. Return to the browser tab containing the Azure portal (or re-open the [Azure portal](https://portal.azure.com) at `https://portal.azure.com` in a new browser tab) and view the contents of the resource group where you deployed the resources used in this exercise.
1. On the toolbar, select **Delete resource group**.
1. Enter the resource group name and confirm that you want to delete it.
