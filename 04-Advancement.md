### **Title: Enhancement to Title Maker Power Apps – Adding Navigation and Form for Data Update**

**Objective:**  
This enhancement allows the user to navigate from the main screen to a second screen via a button, where they can update the Title Maker data stored in a SharePoint list using a form.
---

### **Steps to Implement the Enhancement:**

#### **1. Add a Button for Navigation to the Second Screen**
   - **Step 1:** Open your Title Maker Power Apps project in Power Apps Studio.
   - **Step 2:** On the first screen (main screen), go to the Insert tab and add a **Button**.
   - **Step 3:** Set the button's text to something like "Next" or "Update Title".
   - **Step 4:** To navigate to the second screen when the button is clicked, set the **OnSelect** property of the button to:
     ```PowerApps
     Navigate(Screen2)
     ```
   - **Step 5:** Create a second screen by selecting **Insert > New Screen**. Name it `Screen2`.

#### **2. Create a Form to Update Data in the SharePoint List**
   - **Step 1:** On the newly created second screen (`Screen2`), go to **Insert > Forms > Edit Form** to insert an edit form.
   - **Step 2:** In the Properties pane of the form, click on **Data Source** and select the SharePoint list that stores your Title Maker data.
   - **Step 3:** In the **Fields** section, select the relevant columns from your SharePoint list that you want to allow users to edit.
   - **Step 4:** Set the form’s **Item** property to:
     ```PowerApps
     LookUp(YourSharePointList, ID = SelectedID)
     ```
     This ensures that the form loads the selected item for editing.
     
   - **Step 5:** To submit changes to the SharePoint list, add a **Button** below the form. Set the **OnSelect** property of this button to:
     ```PowerApps
     SubmitForm(Form1)
     ```
     Replace `Form1` with the actual name of your form.

#### **3. Test the Navigation and Data Update Functionality**
   - **Step 1:** Go back to the first screen and preview the app.
   - **Step 2:** Click on the button to navigate to the second screen.
   - **Step 3:** Fill out the form and click the Submit button to update the Title Maker data in the SharePoint list.
   - **Step 4:** Verify the updates in your SharePoint list to ensure the data is being saved correctly.

---
### **Code Snippets:**
```PowerApps
// OnSelect of the Button on the first screen
Navigate(Screen2)

// Form Item Property to load the selected item for editing
LookUp(YourSharePointList, ID = SelectedID)

// Submit Button to save data back to SharePoint
SubmitForm(Form1)
```

Here are the **detailed steps** to build the custom title approval functionality in Power Apps and Power Automate, including all aspects of the process.

---

### **Part 1: Power Apps Configuration**

#### **1.1. Create a Custom Title SharePoint List**
1. **Go to SharePoint** and create a new list called `CustomTitleList`.
2. **Add columns**:
   - `TitleName` (Single line of text): To store the title name.
   - `CreatedBy` (Person or Group): To capture the name of the user who created the title.
   - `Status` (Choice): Set choices as **Pending**, **Approved**, and **Rejected**. Set the default to **Pending**.

#### **1.2. Add a Custom Title Submission Form in Power Apps**
1. **Open Power Apps Studio** and load your existing Title Maker app.
2. **Create a new screen** (let’s call it `CustomTitleScreen`):
   - In the left navigation pane, click **Insert > New Screen**.
   - Name this screen `CustomTitleScreen`.
3. **Add an Edit Form** to the new screen:
   - Go to **Insert > Forms > Edit Form**.
   - Set the **Data Source** to `CustomTitleList` (the SharePoint list you just created).
   - In the **Fields** section, select `TitleName` and `CreatedBy` for the user to fill.
4. **Add a Submit Button**:
   - Insert a button below the form, and set its **Text** property to `Submit Custom Title`.
   - Set its **OnSelect** property to:
     ```PowerApps
     SubmitForm(CustomTitleForm)
     ```
   - Replace `CustomTitleForm` with the actual name of the form.
   - This button will save the custom title data to the `CustomTitleList` in SharePoint.

#### **1.3. Add Navigation Buttons**
1. **Add a Button** on your **Main Screen** and the **Update Title Screen**:
   - Set its **Text** property to `Create Custom Title`.
   - Set its **OnSelect** property to:
     ```PowerApps
     Navigate(CustomTitleScreen)
     ```
   - This will navigate the user to the screen where they can submit custom titles.

---

### **Part 2: Power Automate Flow Configuration**

#### **2.1. Create a New Flow for Title Approval**
1. **Open Power Automate** and click on **Create > Automated Flow**.
2. **Select a Trigger**:
   - Search for "SharePoint" in the trigger search box.
   - Choose **When an item is created**.
   - Set the **Site Address** to your SharePoint site and the **List Name** to `CustomTitleList`.

#### **2.2. Add Approval Action**
1. **Add a new action**:
   - Click on **New Step** and search for "Approval".
   - Select **Start and wait for an approval**.
2. **Configure the approval**:
   - **Approval Type**: Select **Approve/Reject – First to respond**.
   - **Title**: Set the title of the approval to something like:
     ```PowerAutomate
     Approval request for title: @{triggerOutputs()?['body/TitleName']}
     ```
   - **Assign to**: Enter the email(s) of the approver(s).
   - **Details**: Include details such as the created title and creator:
     ```PowerAutomate
     A new custom title was created by: @{triggerOutputs()?['body/CreatedBy/DisplayName']}. Title: @{triggerOutputs()?['body/TitleName']}
     ```

#### **2.3. Add a Condition for Approval Outcome**
1. **Add a condition**:
   - Click **New Step** and choose **Condition**.
   - In the condition box, set the logic:
     - **If outcome of approval is equal to Approve**:
       ```PowerAutomate
       outputs('Start_and_wait_for_an_approval')?['body/outcome']
       ```
   - In the **If Yes** branch, add actions for approved titles.
   
#### **2.4. Action for Approved Titles**
1. **Create a new item** in the main Title Maker list:
   - Add a **SharePoint - Create Item** action.
   - Set the **Site Address** to your SharePoint site and **List Name** to your main `TitleMakerList`.
   - Map the fields:
     - **TitleName**: Set to `TitleName` from the `CustomTitleList`.
     - **CreatedBy**: Set to the value from the custom title item.
   
2. **Update the original item** in the `CustomTitleList` to reflect the status:
   - Add another **SharePoint - Update Item** action.
   - Set the **ID** to `ID` from the `CustomTitleList`.
   - Set **Status** to **Approved**.

#### **2.5. Action for Rejected Titles**
1. In the **If No** branch of the condition, add an **Update Item** action:
   - Set **Status** to **Rejected** for rejected titles.

---

### **Part 3: Complete Testing and Deployment**

#### **3.1. Test Power Apps**
1. **Preview your app**:
   - Click **Preview** and try navigating between screens using the buttons.
   - Test the custom title submission by filling out the form and clicking the `Submit` button.
   - Ensure that the title is submitted to the `CustomTitleList` in SharePoint.

#### **3.2. Test Power Automate**
1. **Submit a new custom title**:
   - After submitting a title in Power Apps, check your Power Automate flow.
   - Ensure that the approval request is sent, and test both the **Approve** and **Reject** paths.
   - After approval, verify that the title is added to the main `TitleMakerList` and the status is updated to **Approved** in the `CustomTitleList`.

#### **3.3. Finalize**
1. Once all tests are successful, deploy your updated Power Apps and Power Automate flow to production.

---

### **Summary of Steps**
- **In Power Apps**: Create a new screen with a form to submit custom titles. Add navigation buttons on other screens to direct users to this form.
- **In Power Automate**: Create a flow that starts the approval process when a new item is created in the `CustomTitleList`. Based on the approval outcome, update the item or move it to the main `TitleMakerList`.

This solution ensures that only approved custom titles are added to the main data, providing a controlled flow for title creation.

---


