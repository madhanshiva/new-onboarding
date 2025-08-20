# Lab 16: Create a language understanding model with the Language service

### Estimated Duration: 35 Minutes

## Overview

In this lab, you will use the Azure AI Language service to build a conversational language understanding model and integrate it with a simple Python client application. By completing this lab, you will gain hands-on experience in creating, training, and deploying a natural language model that can predict user intent and extract entities.

## Lab Objectives

- **Task 1:** Provision an Azure AI Language resource

- **Task 2:** Create a conversational language understanding project

- **Task 3:** Create intents

- **Task 4:** Label each intent with sample utterances

- **Task 5:** Train and test the model

- **Task 6:** Add entities

- **Task 7:** Retrain the model

- **Task 8:** Use the model from a client app

> **NOTE**
> The task of a conversational language model is to predict the user's intent and identify any entities to which the intent applies. It is <u>not</u> the job of a conversational language model to actually perform the actions required to satisfy the intent. For example, a clock application can use a conversational language model to discern that the user wants to know the time in London, but the client application itself must then implement the logic to determine the correct time and present it to the user.

In this exercise, you'll use the Azure AI Language service to create a conversational language understand model, and use the Python SDK to implement a client app that uses it.

While this exercise is based on Python, you can develop conversational understanding applications using multiple language-specific SDKs; including:

- [Azure AI Conversations client library for Python](https://pypi.org/project/azure-ai-language-conversations/)
- [Azure AI Conversations client library for .NET](https://www.nuget.org/packages/Azure.AI.Language.Conversations)
- [Azure AI Conversations client library for JavaScript](https://www.npmjs.com/package/@azure/ai-language-conversations)

### Task 1: Provision an Azure AI Language resource

In this task, you will provision an Azure AI Language service resource in your subscription and collect the keys and endpoint required to use it

1. Open the Azure portal at `https://portal.azure.com`, and sign in using the Microsoft account.

1. If prompted with a sign-in window, kindly sign in using the provided Azure credentials

    - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

        ![](../Images/l14t1p1.png)

    - **Password:** <inject key="AzureAdUserPassword"></inject>

        ![](../Images/l14t1p2.png)

1. If prompted to **Stay signed in?**, you can click **No**.

    ![](../Images/aifoundrysignin3.png)

1. If a **Welcome to Microsoft Azure** pop-up window appears, simply click **Cancel** to skip the tour.

    ![](../Images/l2at2p2.png)

1. On the Azure Portal home page, select **Create a resource**.

    ![](../Images/AI-l16-1.png)

1. On the **Create a resource** page, type **Language service (1)** in the search box and select **Language service (2)** from the drop down. 

    ![](../Images/AI-l16-2.png)

1. On the **Marketplace** page, under **Language service**, select the **Create (1)** drop-down and then choose **Language service (2)**.

    ![](../Images/AI-l16-3.png)

1. On the **Select additional features** page, click **Continue to create your resource**.

    ![](../Images/AI-l16-4.png)

1. In the Basics tab of **Create Language**, follow these instructions to fill out the properties, then select **Review + create (7)**:

    * Subscription: **Default Subscription (1)**
    * Resource group: **AI-102-RG16 (2)**
    * Region: **<inject key="Region"></inject> (3)**
    * Name: **languageservice<inject key="DeploymentID"></inject> (4)**
    * Pricing tier: Select **F0** (free), or **S** (standard) if F is not available **(5)**.
    * Responsible AI Notice: **Agree (6)**

       ![](../Images/AI-l16-5.png)

1. On the **Review + create** tab, click **Create** to provision the resource.

    ![](../Images/AI-l16-6.png)

1. Wait for deployment to complete, and then click on **Go to resource**.

    ![](../Images/AI-l16-7.png)

1. On the **Languageservice** page, in the left navigation pane, select **Resource Management (1)** > **Keys and Endpoint (2)**. Copy the **Endpoint (3)** and **Key (4)** values, and save them in a notepad file. You will need these details later in the exercise.

    ![](../Images/AI-l16-8.png)

### Task 2: Create a conversational language understanding project

In this task, you will create a new project in Language Studio and define the basic information for your model.

1. In a new browser tab, open the Azure AI Language Studio portal at `https://language.cognitive.azure.com/` and select **Sign in**.
   
    ![](../Images/AI-l16-9.png)

1. If prompted, provide the credentials below:

    - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

    - **Password:** <inject key="AzureAdUserPassword"></inject>

1. If you're prompted to choose a Language resource, select the following settings:

    - **Azure Directory**: The Azure directory containing your subscription **(1)**
    - **Azure subscription**: Your Azure subscription **(2)**
    - **Resource type**: Language **(3)**
    - **Resource name**: Select **languageservice<inject key="DeploymentID" enableCopy="false"/> (4)**
    - Then select **Done (5)**

      ![](../Images/AI-l16-10.png)

       >**Note:** If you are <u>not</u> prompted to choose a language resource, it may be because you have multiple Language resources in your subscription; in which case:
    
        1. On the bar at the top of the page, select the **Settings (&#9881;)** button.
        2. On the **Settings** page, view the **Resources** tab.
        3. Select the language resource you just created, and click **Switch resource**.
        4. At the top of the page, click **Language Studio** to return to the Language Studio home page

1. At the top of the **Language Studio** portal, click **Create new (1)** and select **Conversational language understanding (2)**.  

   ![](../Images/AI-l16-11.png)

1. In the **Create a project** dialog box, on the **Enter basic information** page, enter the following details and then select **Next**:
    - **Name**: `Clock` **(1)**
    - **Utterances primary language**: English **(2)**
    - **Enable multiple languages in project?**: Unselect **(3)**
    - **Description**: `Natural language clock` **(4)**
      
      ![](../Images/AI-l16-12.png)

1. On the **Review and finish** page, select **Create**.

      ![](../Images/AI-l16-13.png)

### Task 3: Create intents

In this task, you will define intents (such as GetTime, GetDay, and GetDate) that represent the user’s goals when submitting utterances.

> **Tip**: When working on your project, if some tips are displayed, read them and select **Got it** to dismiss them, or select **Skip all**.

1. On the **Schema definition** page, under the **Intents** tab, click **Add (1)** to create a new intent.  

    ![](../Images/AI-l16-14.png)

1. On the **Add an intent** page, enter **intent name** as `GetTime` **(1)** and click on **Add intent (2)**.

    ![](../Images/AI-l16-15.png)

1. Verify that the **GetTime** intent is listed (along with the default **None** intent). Then add the following additional intents: 
    - `GetDay`
    - `GetDate`
     
      ![](../Images/AI-l16-16-new.png)

## Task 4: Label each intent with sample utterances

In this task, you will add example utterances to each intent to help the model learn how to predict the correct intent from user input.

1. On the **Data labeling (1)** page, click **Select intent (2)**, then choose **GetTime (3)** from the list and enter the utterance `what is the time?` **(4)**.

    ![](../Images/AI-l16-17.png)

    ![](../Images/AI-l16-17.1.png)
    
    > **Tip**: You can expand the pane with the **>>** icon to see the page names, and hide it again with the **<<** icon.

    ![](../Images/AI-l16-note.png)

1. Add the following additional utterances for the **GetTime** intent:  
    - `what's the time?`
    - `what time is it?`
    - `tell me the time`

        > **NOTE**
        > To add a new utterance, write the utterance in the textbox next to the intent and then press ENTER. 

1. Select the **GetDay (1)** intent and enter the utterance `what day is it?` **(2)**.

   ![](../Images/AI-l16-18.png)

1. Add the following additional utterances for the **GetDay** intent:
    - `what's the day?`
    - `what is the day today?`
    - `what day of the week is it?`

1. Select the **GetDate (1)** intent and enter the utterance `what date is it?` **(2)**.

   ![](../Images/AI-l16-19.png)

1. Add the following additional utterances for the **GetDate** intent:
    - `what's the date?`
    - `what is the date today?`
    - `what's today's date?`

      ![](../Images/AI-l16-18.png)

1. After you've added utterances for each of your intents, select **Save changes**.

   ![](../Images/AI-l16-20.png)

## Task 5: Train and test the model

In this task, you will train the language model on the defined intents and utterances, then test it by submitting sample queries to check prediction accuracy.

1. In the left pane, select **Training jobs (1)** and then click **+ Start a training job (2)**.  

    ![](../Images/AI-l16-21.png)

1. On the **Start a training job** tab, select the following detais and click **Train (4)**
   - Select **Train a new model (1)**  
   - Enter the name **Clock (2)**  
   - Choose **Standard training (3)** mode  
   - Keep the default **Data splitting** options 

       ![](../Images/AI-l16-22.png)

1. Wait for the training process to complete (this may take several minutes). When finished, the job **Status** will change to **Training succeeded**.  

    ![](../Images/AI-l16-23.png)

1. Select the **Model performance (1)** page, and then select the **Clock (2)** model.
   
    ![](../Images/AI-l16-24.png)

1. Review the overall and per-intent evaluation metrics (*precision*, *recall*, and *F1 score*) and the *confusion matrix* generated by the evaluation that was performed when training (note that due to the small number of sample utterances, not all intents may be included in the results).

   ![](../Images/AI-l16-25.png)

   > **NOTE**
    > To learn more about the evaluation metrics, refer to the [documentation](https://learn.microsoft.com/azure/ai-services/language-service/conversational-language-understanding/concepts/evaluation-metrics)

1. Navigate to the **Deploying a model (1)** page and select **Add deployment (2)**.  

   ![](../Images/AI-l16-26.png)

1. On the **Add deployment** dialog, select **Create a new deployment name (1)**, and then enter `production` **(2)**.

1. Select the **Clock (3)** model in the **Model** field then select **Deploy (4)**. The deployment may take some time.

   ![](../Images/AI-l16-27.png)

1. After deployment is complete, go to the **Testing deployments (1)** page and select **production (2)** from the **Deployment name** field.  

1. Enter the following text in the empty textbox **(3)**, and then select **Run the test**:

    `what's the time now?`

    ![](../Images/AI-l16-28.png)

1. Review the result that is returned, noting that it includes the predicted intent (which should be **GetTime**) and a confidence score that indicates the probability the model calculated for the predicted intent. The JSON tab shows the comparative confidence for each potential intent (the one with the highest confidence score is the predicted intent)

   ![](../Images/AI-l16-29.png)

1. Clear the text box, and then run another test with the following text:

    `tell me the time`

    Again, review the predicted intent and confidence score.

     ![](../Images/AI-l16-31.png)

1. Try the following text:

    `what's the day today?`

    Hopefully the model predicts the **GetDay** intent.

     ![](../Images/AI-l16-32.png)

## Task 6 Add entities

So far you've defined some simple utterances that map to intents. Most real applications include more complex utterances from which specific data entities must be extracted to get more context for the intent.

## Task 6.1: Add a learned entity

The most common kind of entity is a *learned* entity, in which the model learns to identify entity values based on examples.

1. In Language Studio, return to the **Schema definition (1)** page and then on the **Entities (2)** tab, select **&#65291; Add (3)** to add a new entity.

   ![](../Images/AI-l16-33.png)

1. In the **Add an entity** dialog box, enter the entity name `Location` **(1)** and ensure that the **Learned (2)** tab is selected. Then select **Add entity (3)**.

    ![](../Images/AI-l16-34.png)

1. After the **Location** entity has been created, return to the **Data labeling** page.

   ![](../Images/AI-l16-35.png)

1. Select the **GetTime (1)** intent and enter the following new example utterance **(2)**:

    `what time is it in London?`

    ![](../Images/AI-l16-36.png)

1. When the utterance has been added, select the word **London (1)**, and in the drop-down list that appears, select **Location (2)** to indicate that "London" is an example of a location.

   ![](../Images/AI-l16-37.png)

1. Add another example utterance for the **GetTime** intent:

    `Tell me the time in Paris?`

1. When the utterance has been added, select the word **Paris (1)**, and map it to the **Location (2)** entity.

    ![](../Images/AI-l16-38.png)

1. Add another example utterance for the **GetTime** intent:

    `what's the time in New York?`

1. When the utterance has been added, select the words **New York (1)**, and map them to the **Location (2)** entity.

   ![](../Images/AI-l16-39.png)

1. Select **Save changes** to save the new utterances.

    ![](../Images/AI-l16-40.png)

## Task 6.2 Add a *list* entity

In some cases, valid values for an entity can be restricted to a list of specific terms and synonyms; which can help the app identify instances of the entity in utterances.

1. In Language Studio, return to the **Schema definition (1)** page and then on the **Entities (2)** tab, select **&#65291; Add (3)** to add a new entity.

    ![](../Images/AI-l16-41.png)

1. In the **Add an entity** dialog box, enter the entity name `Weekday` **(1)** and select the **List (2)** entity tab. Then select **Add entity (2)**.

    ![](../Images/AI-l16-42.png)

1. On the page for the **Weekday** entity, in the **Learned (1)** section, ensure **Not required (2)** is selected.

     ![](../Images/AI-l16-43.png)

1. Then, in the **List (1)** section, select **&#65291; Add new list (2)**. Then enter the following value **(3)** and synonym and select **Save (4)**:

    | List key | synonyms|
    |-------------------|---------|
    | `Sunday` | `Sun` |
    
    ![](../Images/AI-l16-44.png)

    ![](../Images/AI-l16-45.png)
    
    > **NOTE**
    > To enter the fields of the new list, insert the value `Sunday` in the text field, then click on the field where 'Type in value and press enter...' is displayed, enter the synonyms, and press ENTER.

1. Repeat the previous step to add the following list components:

    | Value | synonyms|
    |-------------------|---------|
    | `Monday` | `Mon` |
    | `Tuesday` | `Tue, Tues` |
    | `Wednesday` | `Wed, Weds` |
    | `Thursday` | `Thur, Thurs` |
    | `Friday` | `Fri` |
    | `Saturday` | `Sat` |

1. After adding and saving the list values, return to the **Data labeling (1)** page.

1. Select the **GetDate (2)** intent and enter the following new example utterance **(2)**:

    `what date was it on Saturday?`

    ![](../Images/AI-l16-46.png)

1. When the utterance has been added, select the word **Saturday (1)**, and in the drop-down list that appears, select **Weekday (2)**.

   ![](../Images/AI-l16-47.png)

1. Add another example utterance for the **GetDate** intent:

    `what date will it be on Friday?`

1. When the utterance has been added, map **Friday (1)** to the **Weekday (2)** entity.

    ![](../Images/AI-l16-48.png)

1. Add another example utterance for the **GetDate** intent:

    `what will the date be on Thurs?`

1. When the utterance has been added, map **Thurs (1)** to the **Weekday (2)** entity.

    ![](../Images/AI-l16-49.png)

1. select **Save changes** to save the new utterances.

    ![](../Images/AI-l16-50.png)

## Task 6.3 Add a *prebuilt* entity

The Azure AI Language service provides a set of **prebuilt** entities that are commonly used in conversational applications.

1. In Language Studio, return to the **Schema definition (1)** page and then on the **Entities (2)** tab, select **&#65291; Add (3)** to add a new entity.

   ![](../Images/AI-l16-51.png)

1. In the **Add an entity** dialog box, enter the entity name `Date` **(1)** and select the **Prebuilt (2)** entity tab. Then select **Add entity (3)**.

    ![](../Images/AI-l16-52.png)

1. On the page for the **Date** entity, in the **Learned (1)** section, ensure **Not required (2)** is selected. Then, in the **Prebuilt** section, select **&#65291; Add new prebuilt**.

    ![](../Images/AI-l16-53.png)

1. Then, in the **Prebuilt (1)** section, select **&#65291; Add new prebuilt (2)**.

    ![](../Images/AI-l16-54.png)

1. In the **Select prebuilt** list, select **DateTime (1)** and then select **Save (2)**.

    ![](../Images/AI-l16-55.png)

1. After adding the prebuilt entity, return to the **Data labeling (1)** page

1. Select the **GetDay (2)** intent and enter the following new example utterance **(2)**:

    `what day was 01/01/1901?`

    ![](../Images/AI-l16-56.png)

1. When the utterance has been added, select **01/01/1901 (1)**, and in the drop-down list that appears, select **Date (2)**.

   ![](../Images/AI-l16-57.png)

1. Add another example utterance for the **GetDay** intent:

    `what day will it be on Dec 31st 2099?`

1. When the utterance has been added, map **Dec 31st 2099 (1)** to the **Date (2)** entity.

    ![](../Images/AI-l16-58.png)

1. Select **Save changes** to save the new utterances.

    ![](../Images/AI-l16-59.png)

## Task 7: Retrain the model

Now that you've modified the schema, you need to retrain and retest the model.

1. On the **Training jobs (1)** page, select **Start a training job (2)**.

    ![](../Images/AI-l16-60.png)

1. On the **Start a training job** dialog,  select  **overwrite an existing model (1)** and specify the **Clock (2)** model. Select **Train (3)** to train the model. If prompted, confirm you want to overwrite the existing model.

    ![](../Images/AI-l16-61.png)

    >**Note:** If prompted, confirm you want to **overwrite and train** the existing model.

    ![](../Images/AI-l16-62.png)

1. When training is complete the job **Status** will update to **Training succeeded**.

    ![](../Images/AI-l16-63.png)

1. Select the **Model performance (1)** page and then select the **Clock (2)** model. Review the evaluation metrics (*precision*, *recall*, and *F1 score*) and the *confusion matrix* generated by the evaluation that was performed when training (note that due to the small number of sample utterances, not all intents may be included in the results).

    ![](../Images/AI-l16-64.png)

1. On the **Deploying a model (1)** page, select **Add deployment (2)**.

    ![](../Images/AI-l16-65.png)

1. On the **Add deployment** dialog, select **Override an existing deployment name (1)**, and then select **production (2)**.

1. Select the **Clock (3)** model in the **Model** field and then select **Deploy (4)** to deploy it. This may take some time.

    ![](../Images/AI-l16-66.png)

1. When the model is deployed, on the **Testing deployments (1)** page, select the **production (2)** deployment under the **Deployment name** field, enter the  following text **(3)** and click on **Run the test (4)** :

    `what's the time in Edinburgh?`

    ![](../Images/AI-l16-67.png)

1. Review the result that is returned, which should hopefully predict the **GetTime** intent and a **Location** entity with the text value "Edinburgh".

    ![](../Images/AI-l16-68.png)

1. Try testing the following utterances:

    `what time is it in Tokyo?`

    `what date is it on Friday?`

    `what's the date on Weds?`

    `what day was 01/01/2020?`

    `what day will Mar 7th 2030 be?`

## Task 8: Use the model from a client app

In a real project, you'd iteratively refine intents and entities, retrain, and retest until you are satisfied with the predictive performance. Then, when you've tested it and are satisfied with its predictive performance, you can use it in a client app by calling its REST interface or a runtime-specific SDK.

## Task 8.1: Prepare to develop an app in Cloud Shell

You'll develop your language understanding app using Cloud Shell in the Azure portal. The code files for your app have been provided in a GitHub repo.

1. On the **Azure portal** homepage, click the **\[>\_] Cloud Shell (1)** button located to the right of the **Copilot** tab at the top. This opens a new Cloud Shell session. In the **Welcome to Azure Cloud Shell** window, choose **PowerShell (2)**.

    ![](../Images/AI-l16-69.png)

    ![](../Images/AI-l16-70.png)

    > **Note**: If you have previously created a cloud shell that uses a *Bash* environment, switch it to **PowerShell**.

1. In the **Getting started** window, ensure **No storage account required (1)** is selected. From the **Subscription** drop-down, choose **Default subscription (2)**, then click **Apply (3)**.

    ![](../Images/AI-l16-71.png)

1. In the Cloud Shell toolbar, open the **Settings (1)** menu and choose **Go to Classic version (2)** from the drop-down.

    ![](../Images/AI-l16-72.png)

    **<font color="black">Ensure you've switched to the classic version of the cloud shell before continuing.</font>**

1. In the PowerShell pane, enter the following commands to clone the GitHub repo for this exercise:

    ```
   rm -r mslearn-ai-language -f
   git clone https://github.com/microsoftlearning/mslearn-ai-language
    ```

    > **Tip**: As you paste commands into the cloudshell, the ouput may take up a large amount of the screen buffer. You can clear the screen by entering the `cls` command to make it easier to focus on each task.

1. After the repo has been cloned, navigate to the folder containing the application code files:  

    ```
   cd mslearn-ai-language/Labfiles/03-language/Python/clock-client
    ```

    ![](../Images/AI-l16-73.png)

## Task 8.2: Configure your application

1. In the command line pane, run the following command to view the code files in the **clock-client** folder:

    ```
   ls -a -l
    ```

    ![](../Images/AI-l16-74.png)

    The files include a configuration file (**.env**) and a code file (**clock-client.py**).

1. Create a Python virtual environment and install the Azure AI Language Conversations SDK package and other required packages by running the following command:

    ```
   python -m venv labenv
    ./labenv/bin/Activate.ps1
   pip install -r requirements.txt azure-ai-language-conversations==1.1.0
    ```
1. Enter the following command to edit the configuration file:

    ```
   code .env
    ```
    ![](../Images/AI-l16-75.png)

    The file is opened in a code editor.

1. Update the configuration values to include the  **endpoint** and a **key** from the Azure Language resource you created (available on the **Keys and Endpoint** page for your Azure AI Language resource in the Azure portal).

    ![](../Images/AI-l16-76.png)

1. After you've replaced the placeholders, within the code editor, use the **CTRL+S** command or **Right-click > Save** to save your changes and then use the **CTRL+Q** command or **Right-click > Quit** to close the code editor while keeping the cloud shell command line open.

## Task 8.3: Add code to the application

1. Enter the following command to edit the application code file:

    ```
   code clock-client.py
    ```

   ![](../Images/AI-l16-77.png)

1. Review the existing code. You will add code to work with the AI Language Conversations SDK.

    > **Tip**: As you add code to the code file, be sure to maintain the correct indentation.

1. At the top of the code file, under the existing namespace references, find the comment **Import namespaces** and add the following code to import the namespaces you will need to use the AI Language Conversations SDK:

    ```python
   # Import namespaces
   from azure.core.credentials import AzureKeyCredential
   from azure.ai.language.conversations import ConversationAnalysisClient
    ```

    ![](../Images/AI-l16-78.png)

1. In the **main** function, note that code to load the prediction endpoint and key from the configuration file has already been provided. Then find the comment **Create a client for the Language service model** and add the following code to create a conversation analysis client for your AI Language service:

    ```python
   # Create a client for the Language service model
   client = ConversationAnalysisClient(
        ls_prediction_endpoint, AzureKeyCredential(ls_prediction_key))
    ```

    ![](../Images/AI-l16-79.png)

1. Note that the code in the **main** function prompts for user input until the user enters "quit". Within this loop, find the comment **Call the Language service model to get intent and entities** and add the following code:

    ```python
   # Call the Language service model to get intent and entities
   cls_project = 'Clock'
   deployment_slot = 'production'

   with client:
        query = userText
        result = client.analyze_conversation(
            task={
                "kind": "Conversation",
                "analysisInput": {
                    "conversationItem": {
                        "participantId": "1",
                        "id": "1",
                        "modality": "text",
                        "language": "en",
                        "text": query
                    },
                    "isLoggingEnabled": False
                },
                "parameters": {
                    "projectName": cls_project,
                    "deploymentName": deployment_slot,
                    "verbose": True
                }
            }
        )

   top_intent = result["result"]["prediction"]["topIntent"]
   entities = result["result"]["prediction"]["entities"]

   print("view top intent:")
   print("\ttop intent: {}".format(result["result"]["prediction"]["topIntent"]))
   print("\tcategory: {}".format(result["result"]["prediction"]["intents"][0]["category"]))
   print("\tconfidence score: {}\n".format(result["result"]["prediction"]["intents"][0]["confidenceScore"]))

   print("view entities:")
   for entity in entities:
        print("\tcategory: {}".format(entity["category"]))
        print("\ttext: {}".format(entity["text"]))
        print("\tconfidence score: {}".format(entity["confidenceScore"]))

   print("query: {}".format(result["result"]["query"]))
    ```

    ![](../Images/AI-l16-80.png)
    
    The call to the conversational understanding model returns a prediction/result, which includes the top (most likely) intent as well as any entities that were detected in the input utterance. Your client application must now use that prediction to determine and perform the appropriate action.

1. Find the comment **Apply the appropriate action**, and add the following code, which checks for intents supported by the application (**GetTime**, **GetDate**, and **GetDay**) and determines if any relevant entities have been detected, before calling an existing function to produce an appropriate response.

    ```python
   # Apply the appropriate action
   if top_intent == 'GetTime':
        location = 'local'
        # Check for entities
        if len(entities) > 0:
            # Check for a location entity
            for entity in entities:
                if 'Location' == entity["category"]:
                    # ML entities are strings, get the first one
                    location = entity["text"]
        # Get the time for the specified location
        print(GetTime(location))

   elif top_intent == 'GetDay':
        date_string = date.today().strftime("%m/%d/%Y")
        # Check for entities
        if len(entities) > 0:
            # Check for a Date entity
            for entity in entities:
                if 'Date' == entity["category"]:
                    # Regex entities are strings, get the first one
                    date_string = entity["text"]
        # Get the day for the specified date
        print(GetDay(date_string))

   elif top_intent == 'GetDate':
        day = 'today'
        # Check for entities
        if len(entities) > 0:
            # Check for a Weekday entity
            for entity in entities:
                if 'Weekday' == entity["category"]:
                # List entities are lists
                    day = entity["text"]
        # Get the date for the specified day
        print(GetDate(day))

   else:
        # Some other intent (for example, "None") was predicted
        print('Try asking me for the time, the day, or the date.')
    ```
    
    ![](../Images/AI-l16-81.png)

1. Save your changes (CTRL+S), then enter the following command to run the program (you maximize the cloud shell pane and resize the panels to see more text in the command line pane):

    ```
   python clock-client.py
    ```
    ![](../Images/AI-l16-82.png)

1. When prompted, enter utterances to test the application. For example, try:

    *Hello*

    *What time is it?*

    *What's the time in London?*

    *What's the date?*

    *What date is Sunday?*

    *What day is it?*

    *What day is 01/01/2025?*

    > **Note**: The logic in the application is deliberately simple, and has a number of limitations. For example, when getting the time, only a restricted set of cities is supported and daylight savings time is ignored. The goal is to see an example of a typical pattern for using Language Service in which your application must:
    >   1. Connect to a prediction endpoint.
    >   2. Submit an utterance to get a prediction.
    >   3. Implement logic to respond appropriately to the predicted intent and entities.

1. When you have finished testing, enter *quit*.

    ![](../Images/AI-l16-83.png)

## Clean up resources

If you're finished exploring the Azure AI Language service, you can delete the resources you created in this exercise. Here's how:

1. Close the Azure cloud shell pane
1. In the Azure portal, browse to the Azure AI Language resource you created in this lab.
1. On the resource page, select **Delete** and follow the instructions to delete the resource.

## More information

To learn more about conversational language understanding in  Azure AI Language, see the [Azure AI Language documentation](https://learn.microsoft.com/azure/ai-services/language-service/conversational-language-understanding/overview).
