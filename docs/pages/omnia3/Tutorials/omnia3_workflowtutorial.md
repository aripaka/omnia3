---
title: OMNIA 3.0 Workflow Tutorial
keywords: omnia3
summary: "OMNIA Workflow Tutorial"
sidebar: omnia3_sidebar
permalink: omnia3_workflowtutorial.html
folder: omnia3
---

## 1. Introduction

OMNIA's Worflow Tutorial enhances your experience on OMNIA Platform 3.0, combining advanced elements and features from previous Tutorials with your daily workflow tools.

## 2. Prerequisites

This tutorial assumes that you have created a OMNIA tenant, and are logged in as a user with modeling privileges to this tenant.

## 3. Modeling a Workflow apllication

1. Start by selecting the tenant where you are going to model, and you will be redirected to the modeling area.
 
    ![Homepage_Dashboard](http://funkyimg.com/i/2DVGv.png)
 
2. Create a new  **Agent**, and set its  *Code*  to  **Company**.
       
3. Return to ***Agents / Add new***, and set its *Code* as **Employee**.

    ![Modeler_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Modeler-Agent-Employee.PNG)

4. Create a new  **Resource**, and set its  *Code*  to  **Task**.
    
5. Access the option  ***Versioning / Builds / Create new***.

6. Proceed to the Application area, configure all figures previously defined, by selecting the option ***Configurations / Company / Create new***, and defining its *Code* and *Name*.

7. Follow the same process of the previous step to create a new **Employee** and **Task**.

8. Go back to Modeling area and create a new **Event** with *Code* set as **Executedtask**, **Task** as the *resource* to be exchanged, **Employee** as provider agent, and **Company** as receiver agent.

    ![Event_Executed task](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Event-TaskList.PNG)

9. Create a new **Document**, naming it as **TaskReport**.

10. Add a new **Generic Entity**, and set the code as *Project*.

11. On Modeler, go to ***Data Analytics / Queries*** and verify if **Executedtask_Query** is already created.


12. Proceed to ***Analytics / Dashboards*** and create a new list. Set **Executedtasklist** as *Code*, the query created on first step (**ExecutedtaskQuery**) as the *source* of the list and **Task** as *Label*.

    ![Task List Query](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Queries-List-Executedtask.PNG)
    
13. Add new columns to **List**. Add a column for Query Property *Code*, with Label *Task*, and format as *Text*

    ![Task List Query](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Code-QueryList.PNG)

14. On ***Data Analytics / Dashboards***, create a new dashboard and set **Home** as *Code*, so the dashboard will be visible on application’s homepage.

    ![Modeler_Create_Dashboard](https://github.com/numbersbelieve/omnia3/raw/master/docs/tutorialPics/modelingTutorial/Modeler-Create-Dashboard.PNG)
    
15. Add lists to **Dashboard**. Set **Executedtask** as *Code*, **Executedtask_List** (created previously) and position it in the first Row and Column, with Size six.

16. Add an new Attribute to your Document. Set its *Code* as **Project*, *Type* as ***Generic Entity / Project***.

    - Add a new Attribute to your Document. Set its *Code* as **Company**, *Type* as ***Generic Entity / Project*** , and as required by checking option *Is required?*.

    - Add a new Attribute to your Document. Set its *Code* as **Employee**, Type as ***Generic Entity / Project***, and as required by checking option *Is required?*.


17. Edit the previously modeled Document **TaskReport**. Create a new Attribute, setting its *Code* and *Type* to **EmployeeName** and ***Primitive / Text***, respectively. Set the attribute as *Read Only*.

    
    ![Modeler_Create_Dashboard](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Attribute%20-%20EmployeeName.PNG)

18. Add a new **Attribute** to your **TaskReport** Document, and set its *Code* as **SheetID**, *Type* ***Primitive / Text / Is ready only?***   

19. Go to **Google Sheets** and create a new file, setting its *Code* as **Task Project 01**. Then, export it as XLSX document.   

    ![Modeler_Create_Dashboard](https://github.com/numbersbelieve/omnia3/raw/master/docs/tutorialPics/modelingTutorial/Task%20Project%2001.xlsx)

20. Create a new **Action Behaviour** to fill the new attribute (on the **TaskReport** document, go to tab *Behaviours* and click on ***Add new / Action***). Set **GetReportData** as *Code*, **SheetID** as the attribute that triggers the behaviour, and paste the following code:

    ````
    if(!string.IsNullOrEmpty(SheetID)){
    
    var client = new System.Net.Http.HttpClient() { };


    string googleApiKey = "INSERT YOUR KEY HERE";
    string apiEndpoint = $"https://sheets.googleapis.com/v4/spreadsheets/{SheetID}/values/Sheet1?key={googleApiKey}";
    var requestResult = client.GetAsync(apiEndpoint).GetAwaiter().GetResult();

    string responseBody = requestResult.Content.ReadAsStringAsync().Result;

    if (!requestResult.IsSuccessStatusCode)
        throw new Exception("Error on retrieving Google sheet: " + responseBody);

    var sheet = JsonConvert.DeserializeObject<Dictionary<string, object>>(responseBody);



    var values = JsonConvert.DeserializeObject<List<List<string>>>(sheet["values"].ToString());

    foreach (var sheetLine in values)
    {
        Executedtask taskLine = new Executedtask();

        taskLine._resource = sheetLine.ElementAt(0);
        taskLine._quantity = Convert.ToInt32(sheetLine.ElementAt(1));

        ReportLines.Add(taskLine);
                
    }
} 
    ````

21. Create a **new Build**.

22. Go back to **Generic Entity**, and add a new **Attribute** to **Project**. Set its *Code* as ***SheetID / Primitive / Text***.

23. Go to **Events**, and add a new **Attribute** to **Executedtask**. Set its *Code* as *TaskDate* > ***Primitive / Date***.

24. Add another **Attribute** to **Executedtask**, setting its *Code* as ***Description / Primitive / Date***.  

25. Go to ***Application / Configurations*** area, and create a new **Project**. Please observe that, when *Code* is identified (e.g. try with value *SheetID*), is automatically retrieved.

26. Go to **Modeler** area, and add a new element to ***TaskReport / User interface***, setting its *Code* as **Description**.

27. Return to **Application** area, and create a new *Taskreport* **Document**. If **Project** has an associated and valid **Google Sheet**, your **TaskReport** lines will be automatically deployed.







