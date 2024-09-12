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

