### **Step-by-Step Guide: Building "Ericsson Title Maker" in Power Apps**

#### **Step 1: Create a New Canvas App**
1. **Open Power Apps:**
   - Navigate to the [Power Apps](https://make.powerapps.com) portal.

2. **Create a Canvas App from Blank:**
   - Click on **Create** in the left navigation pane.
   - Under **Start from blank**, select **Canvas app from blank**.
  
![image](https://github.com/user-attachments/assets/b2713b97-15b1-437d-ae8a-b078a8986cd6)


3. **Name Your App:**
   - Enter “Ericsson Title Maker” in the app name field.
   - Choose **Create**.

![image](https://github.com/user-attachments/assets/d65a09e2-b2f0-4ca6-b843-0208cdd26dbf)

#### **Step 2: Add Data Source**
1. **Go to Data Panel:**
   - On the left pane, click on **Data**.

2. **Connect to SharePoint:**
   - Click **Add data** and search for SharePoint.
   - Select **SharePoint** and then connect to your SharePoint list.

   ![Add Data Source](link_to_image_3)  
   *(Screenshot showing the "Data" panel and the "Add data" option with SharePoint selected.)*

#### **Step 3: Add Drop-down Controls**
1. **Insert Drop-downs:**
   - Go to the **Insert** tab at the top of the screen.
   - Select **Dropdown** from the controls menu.
   - Insert three dropdowns and rename them as follows:
     - `ddlProduct`
     - `ddlIssue`
     - `ddlError`

   ![Insert Dropdowns](link_to_image_4)  
   *(Screenshot showing the insertion of dropdown controls and renaming them.)*

#### **Step 4: Set Up Drop-downs**
1. **Product Drop-down (`ddlProduct`):**
   - Set the **Items** property to:
     ```powerapps
     Distinct(YourSharePointListName, Product)
     ```
   - Enable **Allow Search** from the dropdown’s properties panel.

   ![Product Drop-down Setup](link_to_image_5)  
   *(Screenshot showing the configuration of `ddlProduct` with the Items property and Allow Search enabled.)*

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

   ![Issue Drop-down Setup](link_to_image_6)  
   *(Screenshot showing the configuration of `ddlIssue` with the Items and Visible properties.)*

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

   ![Error Drop-down Setup](link_to_image_7)  
   *(Screenshot showing the configuration of `ddlError` with the Items and Visible properties.)*

#### **Step 5: Add a Button to Copy and Clear**
1. **Insert Button:**
   - Go to the **Insert** tab, select **Button**, and name it `btnCopy`.
   - Set the **Text** property of the button to “Copy and Clear”.

   ![Insert Button](link_to_image_8)  
   *(Screenshot showing the insertion of a button and setting its text property.)*

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

   ![Configure Button](link_to_image_9)  
   *(Screenshot showing the `OnSelect` property with the given formula.)*

#### **Step 6: Test the App**
1. **Save and Preview:**
   - Click on **File** > **Save** to save your work.
   - Then, click on **Play** (►) in the upper-right corner to preview the app.

2. **Test Functionality:**
   - Select a **Product**, then choose the corresponding **Issue** and **Error**.
   - Click on the **Copy and Clear** button.
   - Ensure that the title is copied to the clipboard and that the dropdowns are reset.

   ![Testing the App](link_to_image_10)  
   *(Screenshot showing the app in preview mode with selections made and the Copy and Clear button pressed.)*

---

### **How to Add Images to GitHub**
To upload these images to your GitHub repository, follow these steps:

1. **Create a Folder in Your Repository:**
   - Navigate to your GitHub repository.
   - Create a new folder called `images` (or a similar name) in the repository.
   
2. **Upload Images:**
   - Drag and drop your screenshots into this folder on GitHub.

3. **Link Images in Markdown:**
   - Use the following Markdown syntax to add images to your documentation:
     ```markdown
     ![Alt Text](relative_path_to_image)
     ```
   - Example:
     ```markdown
     ![Power Apps Create Screen](images/image1.png)
     ```

---

You can take screenshots at each step using the descriptions provided, then add them to your GitHub documentation following the instructions above.
