<script>
    import { Link, navigate } from "svelte-routing";
    import { account } from "../Appwrite.svelte";

    export let created;

    function login() {
        let email = document.getElementById('email').value;
        let password = document.getElementById('password').value;

        account.createEmailSession(email, password).then((res) => {
            navigate('/')
        }).catch((res) => {
            alert("Login failed!")
        });
    }
</script>
{#if created === 'created'}
    <span>Account created successfully. Log in below.</span>
{/if}
<fieldset>
    <legend>Log in</legend>
    <label for="email">Email</label>
    <input type="email" id="email" name="email"><br>
    <label for="password">Password</label>
    <input type="password" id="password" name="password"><br>
    <button on:click={login}>Log in</button> <span>Don't have an account? <Link to="/register">Register</Link></span>
</fieldset>