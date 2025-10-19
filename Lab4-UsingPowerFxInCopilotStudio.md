# Lab 4 - Using Power Fx in Copilot Studio

In this lab, we will look at how Power Fx can be used within the context of [Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/fundamentals-what-is-copilot-studio) and [custom AI Builder prompts](https://learn.microsoft.com/en-us/ai-builder/prompts-overview).

## Scenario

It has been six months since you deployed your Power Apps solution to manage Contacts within Wingtip Toys. The business has been very happy with the solution, but they are now more actively exploring generative AI capabilities and how these can be integrated into the solution you built.

You have identified that a good starting point would be to build a custom AI agent using Copilot Studio that can answer questions about the Contacts stored in the Dataverse table. In addition, the business would like to generate email drafts to send to Contacts based on their birthday. Using Power Fx, you will be able to not only to detect when it is the contact's current birthday and [set this to a variable value](https://learn.microsoft.com/en-us/microsoft-copilot-studio/advanced-power-fx), but also use [dynamic Power Fx expressions](https://learn.microsoft.com/en-us/ai-builder/add-inputs-prompt#power-fx-input) to identify the current day and provide the contact with some interesting facts relating to their birthday. Power Fx will be a key component in allowing us to achieve all of this, and more.

## Instructions

In this lab, you will do the following:

- Create a custom AI agent using Copilot Studio.
- Add a custom topic that can manually generate an email draft to send to a Contact, based on their birthday.
- Create a custom prompt that can be orchestrated from the topic created above, to handle the email draft generation.
- Finalise the topic configuration by taking the email draft and sending it (for the purposes of this lab, we will send the email to our own account).
- Test agent and custom prompt, before publishing into the demo website.

This lab will take approximately 30 minutes to complete.

## Exercise 1: Create a Custom AI Agent

1. Navigate to the [Power Apps Maker Portal](https://make.powerapps.com) and, if not already selected, navigate to the developer environment you created in Lab 0:
   
    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_1.png)

2. Click on **Solutions** from the left-hand navigation menu and then click on the **Wingtip Toys PP Solution** solution you created in Lab 0:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_2.png)

3. In the solution view, click on **+ New**, select **Agent** and then **Agent**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_3.png)

4. Copilot Studio will open in a new browser tab. If a **Welcome to Copilot Studio** dialog appears, click on **Skip**. 

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_4.png)

5. On the **Start building your agent** page, click on **Configure**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_5.png)

6. Enter the following details and then click on **Create**:
    - Name: `Contact Management Agent`
    - Description: `Agent to help sales people manage contacts within the organization.`
    - Instructions: `You are an agent designed to assist with queries relating to Contacts in the organization. You provide responses based on the knowledge sources you have access to. If a question relates to an unrelated topic, you will politely, but firmly, refuse to answer. Hallucinations will not be tolerated.`

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_6.png)

7. After a few moments, the agent will be created and you will be taken to the **Overview** page.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_7.png)

8. It's not possible to add Dataverse as a knowledge source when creating the agent, so we will add this next. Click on **Knowledge** and then **+ Add knowledge**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_8.png)

9. On the **Add knowledge** page, select **Dataverse**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_9.png)

10. Select the **Contact** table and then click on **Add to agent**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_10.png)

11. Verify that the Contact table has been added as a knowledge source:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_11.png)

12. Let's test the agent. If the **Test your agent** pane is not already open, click on **Test**. Then, click on the **Refresh** icon to ensure the latest knowledge is loaded. In the text input box, enter the following question and then press **Enter** or click on the **Send** icon:

    ```
    How many contacts are stored in the system?
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_12.png)

13. After a few moments, the agent should respond with an answer based on the data stored in the Contact table. The response you receive may differ from the below example:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E1_13.png)

14. Experiment with asking a few more questions about the Contacts stored in the Dataverse table to see how the agent responds. Here are some example questions you can try:
    - List the top 5 Contacts whose name begins with the letter 'B'.
    - What is the email address of the contact named 'Adlai Mcall'?
    - How many contacts were created in the last 30 days?

15. Leave the **Contact Management Agent** open if you plan to continue to Exercise 2. Otherwise, you can close the browser tab.

## Exercise 2: Add a Custom Topic

1. If you closed the **Contact Management Agent** in Exercise 1, navigate back to the [Power Apps Maker Portal](https://make.powerapps.com), open the **Wingtip Toys PP Solution** solution and then open the **Contact Management Agent**.

2. On the **Contact Management Agent** page, click on **Topics** and then **+ Add a topic** -> **From blank**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_1.png)

3. Rename the new topic to `Generate Birthday Email`:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_2.png)

4. Provide a description for the topic in the **Describe what the topic does** input:

    ```
    Topic that an agent can invoke to generate a birthday email draft based on the birthday from a Contact, that has been generated from an MCP tool. The topic should also be triggered if no birthday is found for the requested Contact.
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_3.png)

5. Click on **Save** to save your changes so far:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_4.png)

6. Click on the **+** icon underneath the trigger, select **Add a tool**, click **Tool** and then select **Add a tool**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_5.png)

7. In the **Add a tool** pane, select **Model Context Protocol** and then click on **Dataverse MCP Server**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_6.png)

8. If a connection is not automatically created, click the chevron and then select **Create new connection**. Login using your lab environment credentials. Once a connection has been successfully established, click on **Add to agent**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_7.png)

9. Verify that the tool was successfully added to your agent:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_8.png)

10. With our topic, knowledge sources and MCP server defined, the topic will successfully trigger, but we need a mechanism to feed in two key information points - the name and the birthday of the Contact. We will configure two input variables to support this. On the **Generate Birthday Email** topic page, click on the **Details** icon:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_9.png)

11. On the **Topic details** pane, click on **Input**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_10.png)

12. Click on **Create a new variable**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_11.png)
    
13. Configure the input variable as follows. You may need to expand the **Additional settings** section to see all options:
    - **Variable name**: `varContactBirthday`
    - **Variable data type**: Select **Date**
    - **Identify as**: Select **Date**
    - **Description**: `The birthday of the Contact.`
    - **Should prompt user**: Uncheck

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_12.png)

14. Repeat steps 12-13 to create a second input variable with the following configuration:
    - **Variable name**: `varContactName`
    - **Variable data type**: Select **String**
    - **Identify as**: Select **Person name**
    - **Description**: `The full name of the Contact from Dataverse.`
    - **Should prompt user**: Uncheck

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_13.png)

15. Close the **Topic details** pane by clicking on the **X** in the top-right corner.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_14.png)

16. Click on **Save** to save your changes so far.
17. On the designer page for the **Generate Birthday Email** topic, click on the **+** icon under the trigger and then select **Add a condition**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_15.png)

18. When we imported the Contact data in an earlier lab, several Contacts _without_ birthdays were imported. We need to configure the condition to accomodate this scenario, using the Power Fx **IsBlank()** function. On the newly added condition, click on the elipses (...) and then select **Change to formula**.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_16.png)

19. On the **Enter or select a value** input, click the elipses (...) icon and then select **Formula**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_17.png)

20. In the formula bar, enter the following formula and then click on **Insert**:

    ```
    !IsBlank(Topic.varContactBirthday)
    ```
    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_18.png)

21. Verify that the condition has been successfully updated:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_19.png)

22. Under the **All other conditions** branch, click on the **+** icon and then select **Send a message**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_20.png)

23. In the **Send a message** action, enter the following message:

    ```
    Hmmm, it seems we don't have a birthday for {Topic.varContactName}​... 😢
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_21.png)

> [!TIP]
> You may need to click away into the designer pane for the message node to recognize the Power Fx expression and for your screen to resemble the above screenshot.

24. Click on the final **+** icon at the bottom of both branches, select **Topic management** and then **End current topic**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_22.png)

25. Your topic should resemble the screenshot below. Click on **Save** to save your changes so far.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_23.png)

26. Under the condition that checks the birthday is not blank, click on the **+** icon and then select **Send a message**.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_24.png)

27. In the **Send a message** action, enter the following message:

    ```
    Great! {Topic.varContactName}'s birthday is 
    {Text(Topic.varContactBirthday, DateTimeFormat.LongDate)}​. Let's see if that's today...
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_25.png)

> [!TIP]
> You may need to click away into the designer pane for the message node to recognize the Power Fx expression and for your screen to resemble the above screenshot.

28. Click on the **+** icon below the message node created in the previous step and then select **Add a condition**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_26.png)

29. Click on the elipses (...) on the newly created condition and then select **Change to formula**.
30. Click on the elipses (...) icon on the **Enter or select a value** input and then select **Formula**.
31. In the formula bar, enter the following formula and then click on **Insert**:

    ```
    Month(Now()) = Month(Topic.varContactBirthday) && Day(Now()) = Day(Topic.varContactBirthday)
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_27.png)

32. Verify that the condition has been successfully updated:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_28.png)

33. Under the **All other conditions** branch, click on the **+** icon and then select **Send a message**.
34. In the **Send a message** action, enter the following message:

    ```
    Today is {Text(Now(), "dd mmmm")}​, which isn't {Topic.varContactName}'s birthday. Come back again when it's {Text(Topic.varContactBirthday, "dd mmmm")}​!
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_29.png)

> [!TIP]
> You may need to click away into the designer pane for the message node to recognize the Power Fx expression and for your screen to resemble the above screenshot.

35. Click on the **+** icon under the condition defined in step 30 and then select **Send a message**.
36. In the **Send a message** action, enter the following message:

    ```
    Wonderful! Then today is the perfect day to send them an email 🥳 Give me a moment...
    ```
    
    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_30.png)

37. Verify that your topic now resembles the screenshot below. Click on **Save** to save your changes so far.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E2_31.png)

38. Leave the topic designer open, as we will continue working with it in Exercise 3.

## Exercise 3: Create a Custom Prompt

1. You should still have the **Generate Birthday Email** topic open from Exercise 2. If not, navigate back to the [Power Apps Maker Portal](https://make.powerapps.com), open the **Wingtip Toys PP Solution** solution and then open the **Contact Management Agent**. From there, open the **Generate Birthday Email** topic.
2. Under the message node created in step 36 of Exercise 2, click on the **+** icon, select **Add a tool** and then select **New prompt**.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_1.png)

3. In the **Create a prompt** dialog, rename the prompt to `Birthday Email Draft Generator`:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_2.png)

4. In the **Instructions** input, paste in the following text:

    ```
    Generate a birthday greeting that can be sent out via email to a Contact for a company called WingTip Toys, that is structured as follows:

    - Take the following first name {Formula} and as part of the initial greeting, generate an interesting fact regarding the name.    
    - Wish {Contact Name} a happy birthday and then proceed to present 1-3 short, interesting facts regarding their current birthday, which is {Contact Birthday}
    - Close the email by thanking {Contact Name} for their continued business

    Output the results as a JSON object, that stores both the email subject (emailSubject) and the text of the email (emailText).
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_3.png)

5. There are several placeholder values in the above text that need to be replaced. First, highlight `{Contact Name}` in the instructions and click on **+ Add content**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_4.png)

6. The placeholder will be replaced with a slash and additional options will appear. Select **Text**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_5.png)

7. In the dialog that appears, populate the fields as follows and then click on **Close**:
    - **Name**: `inptContactName`
    - **Sample data**: `John Smith`

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_6.png)

8. Repeat steps 5-7 to create a second content input to replace `{Contact Birthday}` in the instructions, using the following configuration:
    - **Name**: `inptContactBirthday`
    - **Sample data**: `1990-03-21`

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_7.png)

9. Next, highlight `{Formula}` in the instructions, click on **+ Add content** and then select **Power Fx**.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_8.png)

10. In the formula dialog, enter the following details and then click on **Close**:
    - **Name**: `Get First Name`
    - **Formula**: `First(Split(Input.inptContactName, " "))`

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_9.png)

11. Highlight the final placeholder for `{Contact Name}` in the instructions, delete, press the `/` key, select the **In your prompt** option and then select **inptContactName**:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_10.png)

12. Your prompt instructions should now resemble the screenshot below.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_11.png)

13. Let's proceed to test the prompt to verify it's working as expected. Click on **Test**.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_12.png)

14. After a few moments, the prompt should return a response. The response should look similar to the screenshot below:

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_13.png)

> [!TIP]
> Responses from Large Language Models (LLMs) are non-deterministic, so your response may differ from the above screenshot - in terms of the greeting, the facts returned and so on. As long as the response is in JSON format and contains both an `emailSubject` and `emailText` property, then the prompt is working as expected.

15. Click on **Save** to save your custom prompt and to return back to the topic designer.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_14.png)

16. Your custom prompt should now be visible within the topic designer, but needs to be configured with the correct inputs. For the **inptContactBirthday** input, click on the elipses (...) icon, select **Formula** and then enter the following formula in the formula bar before clicking on **Insert**:

    ```
    Text(Topic.varContactBirthday, "yyyy-mm-dd")
    ```

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_15.png)

17. Repeat step 16 for the **inptContactName** input, but instead, select the **varContactName** variable.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_16.png)

18. Under **Outputs**, click on **Select a variable** and then **Create a new variable**.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_17.png)

19. Select the newly created **Var1** variable and rename it to `varPromptResponse`.

    ![](Images/Lab4-UsingPowerFxInCopilotStudio/E3_18.png)

20. Save your changes so far by clicking on **Save**.
21. Leave the topic designer open, as we will continue working within it in Exercise 4.

## Exercise 4: Finalise Topic Configuration

TBC

## Exercise 5: Test the AI Agent

TBC

**Congratulations, you've finished Lab 4** 🥳