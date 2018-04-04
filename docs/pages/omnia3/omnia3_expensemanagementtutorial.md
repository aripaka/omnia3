---
title: OMNIA Expense Management Tutorial
keywords: omnia3
summary: "Expense Management Tutorial"
sidebar: omnia3_sidebar
permalink: omnia3_expensemanagementtutorial.html
folder: omnia3
---

## 1. Introduction

Based on a practical example of Expenses reporting, this Management Tutorial demonstrates the versatility and usability of OMNIA Platform on the daily cycle of businesses and organizations.


## 2. Prerequisites

This tutorial assumes that you have created a OMNIA tenant, and are logged in as a user with modeling privileges to this tenant.

## 3. Modeling OMNIA Expense Management

1.  Start by selecting the tenant where you are going to model, and you will be redirected to the modeling area.
    
    ![Homepage_Dashboard](http://funkyimg.com/i/2DVGv.png)
    
2.  Through the left side menu, access the option  **Agents**. Create a new  **Agent**,  by clicking the button  **Add new**  on the top right side, and setting its  **Code**  to  **Company**.
    
   
3.  Return to the  **Agents**  list and add a new agent, by clicking on button  **Add new**  and setting its  _Code_  as  **Employee**.

    ![Modeler_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Modeler-Agent-Employee.PNG)
    
4.  Through the left side menu, access the option  **Resources**. Create a new  **Resource**, by clicking the button  **Add new** on the top right side, and setting its  **Code**  to  **Expenses**.
    
5.  Access the option  **Versioning > Builds**  and build the tenant by clicking the button  **Create new**.
    
6.  On the left side menu, select option  **Go to > Application**, and create a new  **Company**  by selecting the option  **Configurations > Company**  and defining its  _Code_  and  _Name_.
    
    ![Application_Create_Agent](https://github.com/numbersbelieve/omnia3/raw/master/docs/tutorialPics/modelingTutorial/Application-Create-Agent.PNG)
    
7.  Follow the same process of the previous step to create a new  **Employee**  and  **Expenses**.
    
8.  Go back to modeling area (by accessing the option  **Go to > Modeler**) and create a new  **Commitment**  with  _Code_  set as  **ExpensesRequest**,  **Expenses**  as the resource to be exchanged,  **Employee**  as provider agent and  **Company**  as receiver agent.
  
    ![Modeler_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Modeler-Commitment-ExpenseRequest.PNG)

9. On the left side menu, select the option **Generic Entity**. Create a new **Entity**, by clicking the button  **Add new**  on the top right side, and setting its  **Code**  to  **_Currency**.
 
10. Access the option **Agent > Company** and add a new attribute, by clicking on button **Add new**. Set its _Code_ as **Basecurrency**, _Type_ as **Generic Entity > _Currency**, and as required by checking option _Is required?_.

    ![Modeler_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Modeler-Company-Basecurrencyattribute.PNG)
 
11. Go back to application area (by accessing the option **Go to > Application** and configure the option  **_Currency**. Set its *Code* as **EUR** and *name* as **Euro**.

     ![Application_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Application-Configurations-Currency.PNG)
        
12. Access the option **Series** and a new Serie, by clicking on button **Add new**. Set its *Code* as **ExpensesReportSerie** and *name* as **Expenses Report Serie**.

    ![Application_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Application-Serie-ExpensesReportSerie.PNG)

13. Add a new document (by accessing the option **Documents** on side menu and clicking on **Add new** button). Set **ExpenseReport** as the document’s *Code*.

    ![Application_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Application-Document-ExpensesReport.PNG)

14. Go back to modeling area (by accessing the option  **Go to > Modeler**) and edit the  **ExpenseReport**  document to simplify its interface. Add a new attribute by clicking on button  **Add new**. Set its  _Code_  as  **Company**,  _Type_  as  **Agent > Company**, and as required by checking option  _Is required?_.

15. Click on button **Add new** to add an **Attribute** to your **Document**. Set its _Code_ as _ExpenseLines_, _Type_ as **Commitment > ExpensesRequest**.

    ![Application_Create_Agent](https://raw.githubusercontent.com/numbersbelieve/omnia3/master/docs/tutorialPics/modelingTutorial/Modeler-Document-Attribute.PNG)

    - Add a new Attribute** to your **Document**. Set its _Code_ as _Currency, _Type_ as **Generic Entity > _Currency**, and as required by checking option *Is required?*.

    - Add a new Attribute** to your **Document**. Set its _Code_ as _Employee, _Type_ as **Agent > Employee**, and as required by checking option *Is required?*.

    - Add a new Attribute** to your **Document**. Set its _Code_ as _DocumentDate, _Type_ as **Primitive > Date**.

    - Add a new Attribute** to your **Document**. Set its _Code_ as _ExpenseDetails, _Type_ as **Commitment > ExpensesRequest1**.

    - Add a new Attribute** to your **Document**. Set its _Code_ as _ExchangeRate, Type as **Primitive > Decimal**, and as required by checking option Is required?

    - Add a new Attribute** to your **Document**. Set its _Code_ as _BaseCurrency, _Type_ as **Primitive > Text**

    - Add a new Attribute** to your **Document**. Set its _Code_ as _TotalAmount, _Type_ as **Primitive > Decimal**, and as required by checking option *Is required?*
    
    - Add a new Attribute** to your **Document**. Set its _Code_ as _ExpenseDate, _Type_ as **Primitive > Date**, and as required by checking option *Is required?*


16. Perform a new Build (by accessing the option **Versioning > Builds** and clicking on button **Create new**)

17. Add a new **Action Behaviour**, in order to return automatically your updated *Exchange Rate*, based on an external API (consider it as an optional step, though). Set *GetRateData* as Code and paste the following code:

            var client = new System.Net.Http.HttpClient() { };

            string apiEndpoint = $"http://data.fixer.io/api/latest?access_key=13854a5cc70cff0901740c1a7ac3c5b3&symbols={Currency}";
            var requestResult = client.GetAsync(apiEndpoint).GetAwaiter().GetResult();

            var responseBody = requestResult.Content.ReadAsStringAsync().Result;
            var rateData = JsonConvert.DeserializeObject<Dictionary<string, object>>(responseBody);

            if (!requestResult.IsSuccessStatusCode)
                throw new Exception("Error on retrieving rate: " + responseBody);

            var value = JsonConvert.DeserializeObject<Dictionary<string, object>>(rateData["rates"].ToString());

            ExchangeRate = Convert.ToDecimal(value["USD"].ToString());

18. Add a new **Finalize Behaviour** to fill _provider and _receiver attributes by accessing the tab Behaviours and clicking the button Add new > Finalizer. Set FinalizeBehaviours as Code and paste the following code:

            ExpenseDetails.ForEach(a => a._receiver = Company);
            ExpenseDetails.ForEach(a => a._provider = Employee);
            TotalAmount = ExpenseDetails.Sum(a => a._amount);    
    
19. Go to your **ExpenseReport** Document User Interface by accessing the respective tab, and reorganize them to simplify the interface. Remove attribute Provider, Receiver and Quantity from **ExpenseDetails** element. At last, remove Code (twice) attribute from Document.

20. Reorganize Rows and Columns, re-establishing the size and position of their attributes:
    - ***Serie***: Row 1, Column 1 and Size 4;
    - ***Number***: Row 1, Column 2 and Size 4;
    - ***Date***: Row 1, Column 3 and Size 4;
    - ***Company***: Row 2, Column 1 and Size 4;
    - ***Employee***: Row 2, Column 2 and Size 4;
    - ***Currency***: Row 2, Column 3 and Size 4;
    - ***Exchange Rate***: Row 3, Column 1 and Size 2;
    - ***ExpenseDate***: Row 3, Column 5 and Size 4;
    - ***Expense Details***: Row 5, Column 1 and Size 12;
    - ***Resource***: Row 1, Column 1 and Size 2;
    - ***Amount***: Row 1, Column 11 and Size 2;
    - ***Total Amount***: Row 8, Column 11 and Size 2.

21. Go to application and validate interface changes by creating a new **ExpenseReport** document. The interface should be equal to the one below:


