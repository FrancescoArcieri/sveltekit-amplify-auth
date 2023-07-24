## Creating a svelte project (in the folder of the project)
npm create svelte@latest (select: demo app | TypeScript | ESLint+Prettier)

npm install

npm run dev -- --open

## Install AWS Amplify 
npm install aws-amplify

npm install -g @aws-amplify/cli   

  or  npm install @aws-amplify/cli 

  or  curl -sL https://aws-amplify.github.io/amplify-cli/install | bash && $SHELL


## Method 1 (CLI):  
amplify configure

amplify init

amplify add auth

amplify push

amplify console

## Method 2 (AWS console):  
1. Select Amplify 
2. create app 
3. Launch studio
4.  Add authentication 
5. deploy 
amplify pull --appId XXXXXXXXXXX --envName staging
amplify push

## Method 3 (Manual): 
create manually in src folder: aws-exports.ts

## add amplify configuration
create file: src/routes/+layout.ts
add following script:  
<script>
 import { Amplify } from 'aws-amplify';
 import awsmobile from '../aws-exports';
 Amplify.configure(awsmobile);
</script>


## Import Amplify Auth 
in file:  src/routes/login/+page.svelte
add:

<script>
import { Auth } from 'aws-amplify';
</script>


## Fix “ReferenceError: global is not defined”
in src/app.html add in the <head:>: 
 <script>
    if (global === undefined) {
      var global = window;
    }
  </script>
  %sveltekit.head%