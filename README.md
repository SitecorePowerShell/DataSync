# DataSync
Starter kit for building data integrations or content migrations.

## Step 1 - Get Data

![image](https://user-images.githubusercontent.com/11169161/61340855-c08aba80-a809-11e9-9d4f-101cf5a2cd98.png)

##### Generate Request Headers (Web Service Only) 
In the event that you need to authenticate to a site prior to being able to call for data, you can leverage this method. The recommendation is to use ```$PSDefaultParameterValues``` to automatically append any necessary headers or other data onto your outbound calls.

##### Source Path (required)
* Web Service - URL to call to retrieve data
* Flat File - Path to the file
* Sql Query - Connection String to source database

Information is retrieved and stored in the ```$sourceItems``` object, to be used in later methods

##### Paging (Web Service Only)
If the data you are retrieving is paged, such as in a REST API, you can tell the import how to get to the next page. The expression should evaluate to another url for the import process to call.

Example: ``` if ($sourceItems.links.next) { $sourceItems.links.next } ```

##### Before Import
```$sourceItems``` is expected to be an array of objects. If you have anything different, you can use an expression here to massage it into the appropriate format.

## Step 2 - Map Data

![image](https://user-images.githubusercontent.com/11169161/61340856-c1235100-a809-11e9-9439-b38abd7ebc93.png)

##### Template (required)
Select the template to use for creating the new item

##### Force Item ID
Provide an expression to use for assigning an Item ID to the new item. This can be useful if you are imported relational data from another system, such as in content migration scenarios.

##### Item Name (required)
Provide an expression to use for assigning the Item Name. 
```New-ItemName``` is included as a module method; this method will take the passed in value, and sanitize it for Sitecore naming requirements.

##### Display Name
Provide an expession to use for the Display Name of the item. This is useful for multi-lingual imports

##### Field-to-Expression mapping (required)
Map template fields to values in the source data. Each field mapping can be its own expression, or can reference a method for more complex mappings.

Spaces in the field names are supported via underscores (Example: Navigation Title --> Navigation_Title)

##### Skip Existing Items
Used in conjunction with ```Item Sync Field``` to determine if existing items should be overwritten or skipped

##### Item Sync Field
Used to determine if the item being imported already exists within Sitecore

##### Item Processing Script
For any additional processing that may be required during the item import, you can provide an expression here. It is recommended that the extra processors be written as methods, and added to the Custom Functions folder.

##### Multi-lingual Imports
You can provided an expression to use for determining the language of the imported item. The process will use the Item Sync Field to determine if the item already exists in the ```en``` language. If the item does not exist, it is created and then a separate version of the item is created in the given language.

## Step 3 - Store Data

##### Destination Path
Based path for the imported items

##### Parent Path Expression
Used on conjunction with the ```Destination Path``` to determine where the imported item should live in the content tree. The resulting expression should return a path, which is then appended to the base path to build the final path for the imported item.

##### Post Processing
Any extra work that needs to be done post process for the script.

## Technical Stuff

![image](https://user-images.githubusercontent.com/11169161/61340854-c08aba80-a809-11e9-9a3e-2504ed964289.png)

The scripting section is delivered with the necessary expressions for executing the imports. If you are leveraging any custom or 
included methods in any of the above fields, the you will need to add those methods to list of imported methods (see screenshot).

## How To Execute

The Data Sync execution links can be found in the PowerShell Toolbox. 

![image](https://user-images.githubusercontent.com/933163/61093064-5b505700-a40e-11e9-927f-ce09af6ba9a5.png)

If you choose to run the "Data Sync - All" command, then it will process all import scripts defined in the module.

If you choose to run the "Data Sync - Single" command, then you will be prompted to choose your import?:

![image](https://user-images.githubusercontent.com/933163/61093069-61dece80-a40e-11e9-98af-d4271a6644ea.png)

In either case, when the import is complete, you are presenting with information on the content that was imported. This information will also include some helpful metrics detailing counts for new and updated items, as well as total run time.

![image](https://user-images.githubusercontent.com/933163/61093075-67d4af80-a40e-11e9-9db8-9cef07d7eb97.png)

## Resources

- Check out [the article](https://xcentium.com/blog/2019/06/15/sitecore-spe---data-sync-extension) written by Aaron Bickle.
