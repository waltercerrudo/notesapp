

# Task 3: Build the Frontend
<a name="module-three"></a>


|  |  | 
| --- |--- |
| **Time to complete** | 10 minutes  | 
| **Get help** |  +  [Troubleshooting Amplify](https://docs.amplify.aws/react/build-a-backend/troubleshooting/)  <br />+  [Troubleshooting common issues using Amplify UI](https://ui.docs.amplify.aws/react/getting-started/troubleshooting)    | 

## Overview
<a name="overview"></a>

You will learn how to use the Amplify UI component library to scaffold out an entire user authentication flow, allowing users to sign up, sign in, and reset their password with just few lines of code. Additionally, you will build an app frontend that allows users to create, update, and delete their notes. They will also be able to upload an image and associate it with a note. 

## What you will accomplish
<a name="what-you-will-accomplish"></a>
+ Install Amplify libraries 
+ Configure your React app to include authentication, data, and storage for the Notes feature 

## Implementation
<a name="implementation"></a>

### Step 1: Install the Amplify libraries
<a name="install-the-amplify-libraries"></a>

You will need two Amplify libraries for your project. The main **aws-amplify library** contains all of the client-side APIs for connecting your app's frontend to your backend and the **@aws-amplify/ui-react** library contains framework-specific UI components. 
+ Use npm to install libraries

  **Open** a new terminal window, **navigate** to you projects folder (**notesapp**), and **run** the following command to install these libraries in the root of the project. 

  ```
  npm install aws-amplify @aws-amplify/ui-react
  ```  
![Terminal showing using npm install.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/npm-install-amplify.png)

### Step 2: Style the App UI
<a name="style-the-app-ui"></a>
+ Update the Notes UI

  On your local machine, navigate to the ****notesapp/src/index.css**** file, and **update** it with the following code to set the style of the Notes UI. Then, **save** the file. 

  ```
  :root {
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
    line-height: 1.5;
    font-weight: 400;
    color: rgba(255, 255, 255, 0.87);
    font-synthesis: none;
    text-rendering: optimizeLegibility;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem;
  }
  
  .card {
    padding: 2em;
  }
  
  .read-the-docs {
    color: #888;
  }
  
  .box:nth-child(3n + 1) {
    grid-column: 1;
  }
  
  .box:nth-child(3n + 2) {
    grid-column: 2;
  }
  
  .box:nth-child(3n + 3) {
    grid-column: 3;
  }
  ```  
![File directory structure of a "notesapp" project, with "App.jsx" highlighted in the "src" folder.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/file-directory-structure-notesapp-project-1.png)

### Step 3: Implement the UI flow for Notes feature
<a name="implement-the-ui-flow-for-notes-feature"></a>

In this step, you will update the **src/App.jsx** to configure the Amplify library with the client configuration file (**amplify\_outputs.json**). Then, it will generate a data client using the **generateClient()** function. The code uses the Amplify Authenticator component to scaffold out an entire user authentication flow allowing users to sign up, sign in, reset their password, and confirm sign-in for multifactor authentication (MFA).

Additionally, the code contains the following: 
+ **fetchNotes** - Use the data client to list the items in the Notes model. 
+ **createNote** - Get the data from the form and use the data client to create a new note if the user selects an image. Then, the function will upload this image to Amplify storage and associate it with the new note. 
+ **deleteNote** - Use the data client to delete the selected note. 

1. Update the Amplify Authenticator component

   On your local machine, navigate to the **notesapp/src/App.jsx** file, and **update** it with the following code. 

   ```
   import { useState, useEffect } from "react";
   import {
     Authenticator,
     Button,
     Text,
     TextField,
     Heading,
     Flex,
     View,
     Image,
     Grid,
     Divider,
   } from "@aws-amplify/ui-react";
   import { Amplify } from "aws-amplify";
   import "@aws-amplify/ui-react/styles.css";
   import { getUrl } from "aws-amplify/storage";
   import { uploadData } from "aws-amplify/storage";
   import { generateClient } from "aws-amplify/data";
   import outputs from "../amplify_outputs.json";
   
   /**
    * @type {import('aws-amplify/data').Client<import('../amplify/data/resource').Schema>}
    */
   
   Amplify.configure(outputs);
   const client = generateClient({
     authMode: "userPool",
   });
   
   export default function App() {
     const [notes, setNotes] = useState([]);
   
     useEffect(() => {
       fetchNotes();
     }, []);
   
     async function fetchNotes() {
       const { data: notes } = await client.models.Note.list();
       await Promise.all(
         notes.map(async (note) => {
           if (note.image) {
             const linkToStorageFile = await getUrl({
               path: ({ identityId }) => `media/${identityId}/${note.image}`,
             });
             console.log(linkToStorageFile.url);
             note.image = linkToStorageFile.url;
           }
           return note;
         })
       );
       console.log(notes);
       setNotes(notes);
     }
   
     async function createNote(event) {
       event.preventDefault();
       const form = new FormData(event.target);
       console.log(form.get("image").name);
   
       const { data: newNote } = await client.models.Note.create({
         name: form.get("name"),
         description: form.get("description"),
         image: form.get("image").name,
       });
   
       console.log(newNote);
       if (newNote.image)
         if (newNote.image)
           await uploadData({
             path: ({ identityId }) => `media/${identityId}/${newNote.image}`,
             data: form.get("image"),
           }).result;
   
       fetchNotes();
       event.target.reset();
     }
   
     async function deleteNote({ id }) {
       const toBeDeletedNote = {
         id: id,
       };
   
       const { data: deletedNote } = await client.models.Note.delete(
         toBeDeletedNote
       );
       console.log(deletedNote);
   
       fetchNotes();
     }
   
     return (
       <Authenticator>
         {({ signOut }) => (
           <Flex
             className="App"
             justifyContent="center"
             alignItems="center"
             direction="column"
             width="70%"
             margin="0 auto"
           >
             <Heading level={1}>My Notes App</Heading>
             <View as="form" margin="3rem 0" onSubmit={createNote}>
               <Flex
                 direction="column"
                 justifyContent="center"
                 gap="2rem"
                 padding="2rem"
               >
                 <TextField
                   name="name"
                   placeholder="Note Name"
                   label="Note Name"
                   labelHidden
                   variation="quiet"
                   required
                 />
                 <TextField
                   name="description"
                   placeholder="Note Description"
                   label="Note Description"
                   labelHidden
                   variation="quiet"
                   required
                 />
                 <View
                   name="image"
                   as="input"
                   type="file"
                   alignSelf={"end"}
                   accept="image/png, image/jpeg"
                 />
   
                 <Button type="submit" variation="primary">
                   Create Note
                 </Button>
               </Flex>
             </View>
             <Divider />
             <Heading level={2}>Current Notes</Heading>
             <Grid
               margin="3rem 0"
               autoFlow="column"
               justifyContent="center"
               gap="2rem"
               alignContent="center"
             >
               {notes.map((note) => (
                 <Flex
                   key={note.id || note.name}
                   direction="column"
                   justifyContent="center"
                   alignItems="center"
                   gap="2rem"
                   border="1px solid #ccc"
                   padding="2rem"
                   borderRadius="5%"
                   className="box"
                 >
                   <View>
                     <Heading level="3">{note.name}</Heading>
                   </View>
                   <Text fontStyle="italic">{note.description}</Text>
                   {note.image && (
                     <Image
                       src={note.image}
                       alt={`visual aid for ${notes.name}`}
                       style={{ width: 400 }}
                     />
                   )}
                   <Button
                     variation="destructive"
                     onClick={() => deleteNote(note)}
                   >
                     Delete note
                   </Button>
                 </Flex>
               ))}
             </Grid>
             <Button onClick={signOut}>Sign Out</Button>
           </Flex>
         )}
       </Authenticator>
     );
   }
   ```  
![File directory structure of a "notesapp" project, with "App.jsx" highlighted in the "src" folder.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/file-directory-structure-notesapp-project-1.png)

1. Open the app

   Select the **Local host link** to open the application.   
![Terminal output showing Vite server running locally with additional instructions.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/terminal-output-vite-server-running.png)

1. Create an account

   Choose the **Create Account** tab, and use the authentication flow to create a new user by entering your **email address** and a **password**. Then, choose **Create Account**.   
![The resource creation interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/resource-creation-interface.png)

1. Get verification code

   You will get a verification code sent to your email. Enter the **verification code** to log in to the app. When signed in, you can start creating notes and delete them.   
![Email verification screen with a confirmation code input field, pre-filled with '123456,' and buttons for 'Confirm' and 'Resend Code.'.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/email-verification-screen-confirmation.png)

1. Push changes to GitHub

   **Open** a new terminal window, **navigate** to your app's root folder (**notesapp**), and **run** the following command to push the changes to GitHub:  

   ```
   git add .
   git commit -m 'the notes app'
   git push origin main
   ```  
![The AWS sign-in interface.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/sign-interface.png)

1. Sign into the console

   **Sign in** to the AWS Management Console in a new browser window, and **open** the AWS Amplify console at [https://console.aws.amazon.com/amplify/apps](https://console.aws.amazon.com/amplify/apps). 

1. View your web app

   AWS Amplify automatically builds your source code and deployed your app at https://...amplifyapp.com, and on every git push your deployment instance will update. Select the **Visit deployed URL** button to see your web app up and running live.   
![The interface controls and buttons.](https://docs.aws.amazon.com/hands-on/latest/build-react-app-amplify-graphql/images/interface-controls-buttons.png)

## Congratulations
<a name="congratulations"></a>

You have now connected your App to the Amplify backend and built a frontend allowing the users to create, edit, and delete notes. Users will also be able to upload an image and associate it with a note. 