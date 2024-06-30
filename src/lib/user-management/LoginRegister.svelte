<script>
  import { signInWithEmailAndPassword, createUserWithEmailAndPassword } from "firebase/auth"

  import { auth } from "../../firebase";
  import { buttonStyles, inputStyles, labelStyles } from "../../style-vars";

  const ERROR_MAP = {
    "auth/email-already-in-use": "Email already in use",
    "auth/invalid-credential": "Invalid credentials",
    "auth/weak-password": "Password should be at least 6 characters",
    "auth/user-not-found": "User not found",
    "auth/missing-password": "Password cannot be empty",
  }

  let loginOrRegister = 'login'
  let errorMessage = null

  // Style classes
  const switchFormTextStyles = "mt-3 text-sm text-slate-900 dark:text-white text-center"

  const getFormData = (e) => {
    const formData = new FormData(e.target);
    const data = {};

    for (const [key, value] of formData.entries()) {
      data[key] = value;
    }

    return data;
  };

  const login = (e) => {
    e.preventDefault()

    const data = getFormData(e)

    signInWithEmailAndPassword(auth, data.email, data.password)
    .then((userCredential) => { 
      const user = userCredential.user;
    })
    .catch((error) => {
      const errorCode = error.code;
      console.log(`errorCode: ${errorCode} - errorMessage: ${errorMessage}`)
      errorMessage = ERROR_MAP[errorCode]
    });
  }

  const register = (e) => {
    e.preventDefault()

    const data = getFormData(e)

    createUserWithEmailAndPassword(auth, data.email, data.password)
    .then((userCredential) => {
      const user = userCredential.user;
    })
    .catch((error) => {
      const errorCode = error.code;
      errorMessage = ERROR_MAP[errorCode]
      console.log(`errorCode: ${errorCode} - errorMessage: ${errorMessage}`)
    });
  }

  const handleSubmit = (e) => {
    e.preventDefault()

    if (loginOrRegister === "login") {
      login(e)
    } else {
      register(e)
    }
  }

</script>

<div class="login-register-form bg-slate-200 dark:bg-slate-800">
  <h2 class="login-register-header text-3xl text-center text-slate-900 dark:text-white">Login</h2>
  <form action="submit" on:submit|preventDefault={handleSubmit}>
    <div class="sm:col-span-4 my-6">
      <label for="email" class={labelStyles}>Email address</label>
      <div class="mt-2">
        <input id="email" name="email" type="email" class={inputStyles} />
      </div>
    </div>

    <div class="sm:col-span-4 my-6">
      <label for="password" class={labelStyles}>Password</label>
      <div class="mt-2">
        <input id="password" name="password" type="password" class={inputStyles} />
      </div>
    </div>

    {#if errorMessage}
      <p class="text-red-500 text-center mb-3">❌ {errorMessage} ❌</p>
    {/if}

    {#if loginOrRegister === "login"}
      <input type="submit" value="Login" class={buttonStyles} />
      <p class={switchFormTextStyles}>
        Don't have an account? 
        <button class="underline font-semibold" on:click={() => loginOrRegister = 'register'}>Register</button>
      </p>
    {:else}
      <input type="submit" value="Register" class={buttonStyles} />
      <p class={switchFormTextStyles}>
        Already have an account? 
        <button class="underline font-semibold" on:click={() => loginOrRegister = 'login'}>Login</button>
      </p>
    {/if}

  </form>
</div>

<style>
  .login-register-form {
    margin: 100px auto;
    width: 50%;
    padding: 35px;
    border-radius: 10px;
    box-shadow: var(--box-shadow);
  }
</style>