---
title: Zoho Catalyst Vue 3 Front-End
description: Learn how to fetch products from Zoho Books to insert or update them
  in the Catalyst Datastore.
image: assets/images/catalyst.svg
categories:
- Zoho Catalyst
- Vuejs
layout: post
keywords:
- Severless
- Nodejs
- API
- Zoho
- Zoho Catalyst
- Vuejs
- Vue
- Front end development

---

1- Initiate a project with the Catalyst CLI to have the configuration for your catalyst Web App
 Verify that you have the the following files correctly configured:
 .catalystsrc
 catalyst.json
 client-package.json
 
 https://catalyst.zoho.com/help/tutorials/authenticationapp/login-cli.html
 
 ```bash
 $ npm install -g zcatalyst-cli
 $ catalyst login
 ```
 You will be automatically redirected to your browser window, where the Zoho Accounts login page will open. 
 If you are not automatically redirected, visit the URL displayed under Visit this URL on this device to log in.
 
 ```bash
 $ catalyst init
 ```
 
 choose client
 choose basic
 give a name to your client
 
 delete main.css
 delete main.js
 delete index.html
 
 ```bash
 $ cd client
 $ npm init vue@latest
 ```
 follow the instructions and choose your options for the Vuejs app
 
 once created you should have your VueJS app ready.
 
 now we will need to modify the ```client-package.json``` generated from the catalyst cli.
 
 add base:'./' to vite config file
 
 add in main ts
 
 declare global {
  interface Window {
    catalyst: any;
  }
}

index.html
<script src="https://static.zohocdn.com/catalyst/sdk/js/3.0.0/catalystWebSDK.js"></script>
    <script src="/__catalyst/sdk/init.js"></script>

now you can access catalyst in all component from window object

const catalyst = window.catalyst;

catalyst.auth.signIn("login");
