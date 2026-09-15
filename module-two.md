

# Task 2: Initialize a Local Amplify App
<a name="module-two"></a>


|  |  | 
| --- |--- |
| **Time to complete** | 10 minutes  | 
| **Requires** | A text editor. Here are a few free ones: +  [Atom](https://atom.io/)  <br />+  [Notepad\+\+](https://notepad-plus-plus.org/)  <br />+  [Sublime](https://www.sublimetext.com/)  <br />+  [Vim](https://www.vim.org/)  <br />+  [Visual Studio Code](https://code.visualstudio.com/)   | 
| **Get help** |  +  [Troubleshooting Amplify](https://docs.amplify.aws/react/build-a-backend/troubleshooting/)  <br />+  [How Amplify works](https://docs.amplify.aws/react/how-amplify-works/)    | 

## Overview
<a name="overview"></a>

Now that you have a React web app, you will use AWS Amplify to configure a cloud backend for the app. AWS Amplify Gen 2 uses a fullstack TypeScript developer experience (DX) for defining backends. Amplify offers a unified developer experience with hosting, backend, and UI-building capabilities and a code-first approach.   

## What you will accomplish
<a name="what-you-will-accomplish"></a>
+ Set up Amplify Auth 
+ Set up Amplify Data 
+ Set up Amplify Storage 

## Implementation
<a name="implementation"></a>

### Step 1: Set up Amplify Auth
<a name="set-up-amplify-auth"></a>

The app uses email as the default login mechanism. When the users sign up, they receive a verification email. 
+ Set auth resource

  By default, your auth resource is configured as shown inside the ****notesapp/amplify/auth/resource.ts**** file. For this tutorial, **keep** the default auth set up as is.   
![The configuration settings interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/configuration-settings-interface.png)

### Step 2: Set up Amplify Data
<a name="set-up-amplify-data"></a>

The app you will be building is a Notes app that will allow users to create, delete, and list notes. This example app will help you learn how to build many popular types of CRUD\+L (create, read, update, delete, and list) applications. 
+ Update authorization rule

  On your local machine, navigate to the ****notesapp/amplify/data/resource.ts**** file and **update** it with the following code. Then, **save** the file. 
  + The following updated code uses a per-owner authorization rule ****allow.owner()**** to restrict the note record’s access to the owner of the record.  
  + Amplify will automatically add an ****owner: a.string()**** field to each note which contains the note owner's identity information upon record creation. 

  ```
  import { type ClientSchema, a, defineData } from '@aws-amplify/backend';
  const schema = a.schema({  
  Note: a
      .model({
        name:a.string(),
        description: a.string(),
        image: a.string(),
      })
      .authorization((allow) => [allow.owner()]),
  });
  export type Schema = ClientSchema<typeof schema>;
  export const data = defineData({
    schema,
    authorizationModes: {
      defaultAuthorizationMode: 'userPool',
    },
  });
  ```  
![File directory view of a project named "notesapp," with the "resource.ts" file under the "data" folder highlighted in red.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/file-directory-view-project-named-notesapp.png)

### Step 3: Set up Amplify Storage
<a name="set-up-amplify-storage"></a>

1. Create a storage folder

   On your local machine, navigate to the ****notesapp/amplify**** folder, and **create** a new folder named ****storage****, and then **create** a file named ****resource.ts**** inside of the new storage folder.   
![File directory structure of a "notesapp" project, with the "storage" folder and its "resource.ts" file highlighted in red.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/file-directory-structure-notesapp-project.png)

1. Configure a storage resource for your app

   Update the ****amplify/storage/resource.ts**** file with the following code to configure a storage resource for your app. Then, **save** the file. 
   + The updated code will set up the access so that only the person who uploads the image can access. The code will use the ****entity\_id**** as a reserved token that will be replaced with the users' identifier when the file is being uploaded.  

   ```
   import { defineStorage } from "@aws-amplify/backend";
   
   export const storage = defineStorage({
     name: "amplifyNotesDrive",
     access: (allow) => ({
       "media/{entity_id}/*": [
         allow.entity("identity").to(["read", "write", "delete"]),
       ],
     }),
   });
   ```

### Step 4: Deploy Amplify Cloud sandbox
<a name="deploy-amplify-cloud-sandbox"></a>

1. Import backend definitions

   On your local machine, navigate to the ****amplify/backend.ts**** file, and **update** it with the following code. Then, **save** the file. 
   + The following code will import the auth, data, and storage backend definitions: 

   ```
   import { defineBackend } from '@aws-amplify/backend';
   import { auth } from './auth/resource';
   import { data } from './data/resource';
   import { storage } from './storage/resource';
   /**
    * @see https://docs.amplify.aws/react/build-a-backend/ to add storage, functions, and more
    */
   defineBackend({
     auth,
     data,
     storage
   });
   ```  
![Interface element requiring manual review.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface.png)

1. Start sandbox environment

   To start your own personal cloud sandbox environment that provides an isolated development space, in a new terminal window, **run** the following command in your apps root folder: 

   ```
   npx ampx sandbox
   ```
   + The sandbox allows you to rapidly build, test, and iterate on a fullstack app. Each developer on your team can use their own disposable sandbox environment connected to cloud resources. You can learn more about it [here](https://docs.amplify.aws/react/deploy-and-host/sandbox-environments/). 

1. Confirm deployment

   Once the cloud sandbox has been fully deployed, your terminal will display a confirmation message.   
![Interface element requiring manual review.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface-1.png)

1. Verify JSON file was added

   The ****amplify\_outputs.json**** file will be generated and added to your project.    
![Interface element requiring manual review.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface-interface-element.png)

## Conclusion
<a name="conclusion"></a>

You used Amplify to configure auth, data, and storage resources. You also started your own cloud sandbox environment. 