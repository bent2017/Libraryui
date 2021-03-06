# LibraryUI

This is the second part of the Library Application: [https://github.com/florae123/LibraryApp](https://github.com/florae123/LibraryApp).

This part consists of a Node.js server and the web UI.
It is built using HTML, CSS, Bootstrap, jQuery, and a Watson Text-To-Speech Service as well as a Watson Conversation Service.

This first part is a backend server running on Java Liberty on Bluemix that connects to a Cloudant NoSQL Database.

  ![Architecture](./images/lib-architecture-node.jpg)

## Deploy to Bluemix

1. Deploy the Java back end to Bluemix [https://github.com/florae123/library-server-java](https://github.com/florae123/library-server-java) and bind it to a Cloudant NoSQL Database.

2. Clone the app to your local environment from your terminal using the following command

    ```
    git clone https://github.com/florae123/Libraryui
    ```

3. You can find the URL of your java library server under **All Apps** on the Bluemix dashboard for the region you used.

    ![dashboard-click](./images/dashboard.png)

    Copy the URL.

    ![url](./images/java-url.png)

    Open the file **manifest.yml** and change the **"LIBRARY_URI"** to the URL of your java library server and add **"/api"** at the end: **https://[YOUR_LIBRARY_SERVER_URL]/api**

    Change the host name *LibraryUI-[myName]* to something unique. (For example, you might replace [myName] with your name.)

    ```
    applications:
    - name: LibraryUI
      host: libraryui-tsuedbro
      env:
        LIBRARY_URI: "https://library-server-tsuedbro.mybluemix.net/api"
      memory: 64M
      instances: 1
    ```

4. Log in to your Bluemix account using the Cloud Foundry CLI tool.

	```
	cf login
	```

5. Push the app to Bluemix.

	```
	cf push LibraryUI
	```

6. Create a Watson Text-To-Speech Service and connect it to the app LibraryUI.

7. Create a Watson Conversation Service and bind it to the app. Launch, and import a workspace using the file **conversation-workspace.json**. Connect it to the app.

    * Select **Conversation** from the Bluemix Catalog in your Browser, make sure the *Free* pricing plan is selected and click **Create**. You will be directed to a view of the service.
    * To bind this service instance to the node.js application, open the **Connections** panel, and click **Create Connection**. Then select the LibraryUI applicaton and click **Connect**. Restage the application when prompted.
    * Open the **Manage** panel and click **Launch tool**.

        ![Launch](./images/launch-conv.png)

    * Under **Create workspace**, click **Import**.
    * Choose the file **conversation-workspace.json** from your local copy of the LibraryUI directory, select **Everything (Intents, Entities, and Dialog)**, and click **Import**.

        ![import](./images/import-workspace-2.png)

    * Go **back to workspaces** and click **View Details** on the Libray Helper workspace.

        ![details](./images/workspace-id.png)

    * Copy the **Workspace ID** to the clipboard.
    * Replace it with the current value for *workspace_id_copy* in line 38 in **server.js**.

        ```
        //authenticate conversation service
        var workspace_id_copy = 'YOUR_WORKSPACE_ID';
        ```

    * Redeploy the application by repeating step 5: ```cf push LibraryUI```
