## Creating a svelte project (in the folder of the project)
npm create svelte@latest (select: demo app | TypeScript | ESLint+Prettier)
npm install
npm run dev -- --open

## Install AWS Amplify 
npm install aws-amplify

npm install -g @aws-amplify/cli 
or  npm install @aws-amplify/cli 
or  curl -sL https://aws-amplify.github.io/amplify-cli/install | bash && $SHELL

## metodo 0. 
create manually in src folder: aws-exports.ts

## Metodo 1. CLI 
amplify configure
amplify init
amplify add auth
amplify push
amplify console

## Metodo 2. da AWS console 
- Select Amplify 
- create app 
- Launch studio
- Add authentication 
- deploy 
amplify pull --appId dupey3onmcdne --envName staging
amplify push

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