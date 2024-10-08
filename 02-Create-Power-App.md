### **Step-by-Step Guide: Building "Ericsson Title Maker" in Power Apps**

#### **Step 1: Create a New Canvas App**
1. **Open Power Apps:**
   - Navigate to the [Power Apps](https://make.powerapps.com) portal.

2. **Create a Canvas App from Blank:**
   - Click on **Create** in the left navigation pane.
   - Under **Start from blank**, select **Canvas app from blank**.
  
![image](https://github.com/user-attachments/assets/b2713b97-15b1-437d-ae8a-b078a8986cd6)


3. **Name Your App:**
   - Enter “Title Maker” in the app name field.
   - Choose **Create**.

![image](https://github.com/user-attachments/assets/d65a09e2-b2f0-4ca6-b843-0208cdd26dbf)

#### **Step 2: Add Data Source**
1. **Go to Data Panel:**
   - On the left pane, click on **Data**.

2. **Connect to SharePoint:**
   - Click **Add data** and search for SharePoint.
   - Select **SharePoint** and then connect to your SharePoint list.

![image](https://github.com/user-attachments/assets/1bfd1da4-fb67-4e3e-99d0-c10b84e058a9)


#### **Step 3: Add Drop-down Controls**
1. **Insert Drop-downs:**
   - Go to the **Insert** tab at the top of the screen.
   - Select **Dropdown** from the controls menu.
   - Insert three dropdowns and rename them as follows:
     - `ddlProduct`
     - `ddlIssue`
     - `ddlError`

![image](https://github.com/user-attachments/assets/128f0bad-bdf2-4300-a393-0089236b4057)


### **Note:- If you want Searchable drop down select the Combo Box**

#### **Step 4: Set Up Drop-downs**
1. **Product Drop-down (`ddlProduct`):**
   - Set the **Items** property to:
     ```powerapps
     Distinct(YourSharePointListName, Product)
     ```
   - Enable **Allow Search** from the dropdown’s properties panel.

![image](https://github.com/user-attachments/assets/1b3804fd-0062-4b0f-8328-344d86451926)


2. **Issue Drop-down (`ddlIssue`):**
   - Set the **Items** property to:
     ```powerapps
     Distinct(Filter(YourSharePointListName, Product = ddlProduct.Selected.Value), Issue)
     ```
   - Enable **Allow Search**.
   - Set the **Visible** property to:
     ```powerapps
     !IsBlank(ddlProduct.Selected.Value)
     ```

![image](https://github.com/user-attachments/assets/0d756b6e-8fa6-45fd-935b-b2be484caa22)


3. **Error Drop-down (`ddlError`):**
   - Set the **Items** property to:
     ```powerapps
     Distinct(Filter(YourSharePointListName, Product = ddlProduct.Selected.Value && Issue = ddlIssue.Selected.Value), Error)
     ```
   - Enable **Allow Search**.
   - Set the **Visible** property to:
     ```powerapps
     !IsBlank(ddlIssue.Selected.Value)
     ```

![image](https://github.com/user-attachments/assets/709e44b3-00d4-4953-a173-bbb7a308d62d)


#### **Step 5: Add a Button to Copy and Clear**
1. **Insert Button:**
   - Go to the **Insert** tab, select **Button**, and name it `btnCopy`.
   - Set the **Text** property of the button to “Copy and Clear”.

![image](https://github.com/user-attachments/assets/6303a51d-5fe9-470f-8595-f807661d54af)


2. **Configure Button’s `OnSelect` Property:**
   - Set the `OnSelect` property to the following formula:
     ```powerapps
     If(
         !IsBlank(ddlProduct.Selected.Value) && !IsBlank(ddlIssue.Selected.Value),
         Set(
             varTitle,
             Concatenate(
                 "Product: ", ddlProduct.Selected.Value,
                 " Issue: ", ddlIssue.Selected.Value,
                 If(!IsBlank(ddlError.Selected.Value), " Error: " & ddlError.Selected.Value, "")
             )
         );
         Copy(varTitle);
         Notify("Copied to clipboard: " & varTitle, NotificationType.Success);
         Reset(ddlProduct);
         Reset(ddlIssue);
         Reset(ddlError)
     )
     ```


#### **Step 6: Test the App**
1. **Save and Preview:**
   - Click on **File** > **Save** to save your work.
   - Then, click on **Play** (►) in the upper-right corner to preview the app.

2. **Test Functionality:**
   - Select a **Product**, then choose the corresponding **Issue** and **Error**.
   - Click on the **Copy and Clear** button.
   - Ensure that the title is copied to the clipboard and that the dropdowns are reset.

![image](https://github.com/user-attachments/assets/6cc7159a-cba1-441b-9e7e-9e78cfeca0cf)



