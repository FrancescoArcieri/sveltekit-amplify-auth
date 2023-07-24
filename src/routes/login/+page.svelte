<script lang="ts">
	import { Auth } from 'aws-amplify';

	let loggedIn = false;
	let registering = false;

	let username = '';  
	let psw = '';  
	let code = ''; // validation code 

    console.log("Login start");

	const signIn = async (username: string, password: string) => {
		try {
            console.log("signIn clicked");
			console.log(username, password);
			const user = await Auth.signIn(username, password);
			console.log(user);
			loggedIn = true;
			return user;
		} catch (error) {
			console.error(error);
		}
	};

	async function signOut() {
    try {
      await Auth.signOut();
      console.log('User logged out');
      loggedIn = false;
      // Redirect or perform other actions on successful logout
    } catch (error) {
      console.log('Error:', error);
      // Handle logout error
    }
  }

    const signUp = async (username: string, password: string) => {
		try {
			console.log("Signup clicked");
			console.log(username, password);
			const { user } = await Auth.signUp({
				username, 
				password, 
				autoSignIn: { // optional - enables auto sign in after user is confirmed
                   enabled: true,
                }
			});
			console.log(user);
			registering = true;
			return user;
		} catch (error) {
			console.log('error signing up:', error);
		}
	};

	async function confirmSignUp(username, code) {
  try {
    await Auth.confirmSignUp(username, code);
	loggedIn = true;
  } catch (error) {
    console.log('error confirming sign up', error);
  }
}

	const getCurrentUser = async () => {
		try {
            console.log("getCurrentUser clicked");
			const user = await Auth.currentAuthenticatedUser({
				bypassCache: false,
			});
			console.log(user);
			console.info(JSON.stringify(user));
			return user;
		} catch (error) {
			console.error(error);
		}
	};

	async function resendConfirmationCode(username) {
  try {
    await Auth.resendSignUp(username);
    console.log('code resent successfully');
  } catch (err) {
    console.log('error resending code: ', err);
  }
}

</script>


<div class="container">

	{#if loggedIn}
	<div>
	  <p>Welcome, you are logged in!</p>
	  <button on:click={() => getCurrentUser()}>GetUser</button>
	  <button on:click={signOut}>Logout</button>
	</div>


  {:else}
      {#if registering}
	  <label for="code"><b>Code</b></label>
	  <input type="text" placeholder="Enter code" name="code" required bind:value={code} />
	  <button on:click={() => confirmSignUp(username, code)}>Verify code</button>
	  <button on:click={() => resendConfirmationCode(username)}>Resend code</button>
      
	  {:else}
  <div>
	<label for="uname"><b>Username</b></label>
	<input type="text" placeholder="Enter Username" name="uname" required bind:value={username} />

	<label for="psw"><b>Password</b></label>
	<input type="password" placeholder="Enter Password" name="psw" required bind:value={psw} />

	<button type="submit" on:click={() => signIn(username, psw)}>Login</button>
    <button type="submit" on:click={() => signUp(username, psw)}>Register</button>
  </div>
  {/if}
{/if}
		
</div>