Introduction:
This document will provide you with a programmer-oriented walkthrough of using the client_context package to interact with SharePoint in Office 365. By leveraging this package, you can perform various operations, such as reading and writing data, managing lists and libraries, and collaborating with SharePoint sites.

1. Installation:
To start using the Office365-REST-Python-Client package, you need to install it in your Python environment. Use pip, the package manager for Python, to install the package by running the following command:
```
pip install Office365-REST-Python-Client
```

Alternatively the latest version could be directly installed via GitHub:
```
pip install git+https://github.com/vgrem/Office365-REST-Python-Client.git
```

2. Authentication Credentials
For the following examples, relevant credentials can be found in the Azure Portal.
Steps to access:

    a. Login to the home page of the [Azure Portal](https://portal.azure.com/#home)
    b. Navigate to "Azure Active Directory" using the three bars in the top right corner of the portal
    c. Select "App registrations" in the navigation panel on the left
    d. Search for and select your relevant application
    e. In the application's "Overview" page, the client id can be found under "Application (client) id"
    f. In the application's "Certificates & Secrets" page, the client secret can be found under the "Value" of the "Client Secrets." If there is no client secret yet, create one here.

3. Establishing Connection:
Before interacting with SharePoint, you must establish a connection to the SharePoint site. Follow these steps:

    a. Import Required Modules:
    In your Python script or program, import the necessary modules from the office365.sharepoint package:
    ```python
    from office365.sharepoint.client_context import ClientContext
    from office365.runtime.auth.user_credential import UserCredential
    ```

    b. Create Client Context:
    Initialize a ClientContext object with the SharePoint site URL and user credentials:
    ```python
    site_url = "https://your-sharepoint-site-url"
    username = "your-username"
    password = "your-password"
    credentials = UserCredential(username, password)
    ctx = ClientContext(site_url).with_credentials(credentials)
    ```

4. Working with SharePoint Data:
Now that you have a connection, you can interact with SharePoint data using the client_context package. Here are some common operations:

    a. Reading Data:
    Retrieve items from a SharePoint list or library:
    ```python
    list_title = "your-list-title"
    items = ctx.web.lists.get_by_title(list_title).get_items()
    ctx.load(items)
    ctx.execute_query()
    for item in items:
        print(item.properties)
    ```

    b. Creating Items:
    Add new items to a SharePoint list:
    ```python
    list_title = "your-list-title"
    list_obj = ctx.web.lists.get_by_title(list_title)
    item_info = {'Title': 'New Item', 'Description': 'Some description'}
    list_obj.add_item(item_info)
    ctx.execute_query()
    ```

    c. Updating Items:
    Modify properties of an existing item in a SharePoint list:
    ```python
    list_title = "your-list-title"
    list_obj = ctx.web.lists.get_by_title(list_title)
    item_id = 1  # ID of the item to update
    item = list_obj.get_item_by_id(item_id)
    item.set_property('Title', 'Updated Title').update()
    ctx.execute_batch()
    ```

    d. Deleting Items:
    Remove an item from a SharePoint list:
    ```python
    list_title = "your-list-title"
    list_obj = ctx.web.lists.get_by_title(list_title)
    item_id = 1  # ID of the item to delete
    item = list_obj.get_item_by_id(item_id).delete_object().execute_query()
    ```

