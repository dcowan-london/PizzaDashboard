<script>
    import { Link, navigate } from "svelte-routing";
    import { account } from "../Appwrite.svelte";

    function register() {
        let name = document.getElementById('name').value;
        let email = document.getElementById('email').value;
        let password = document.getElementById('password').value;

        account.create('unique()', email, password, name).then((res) => {
            navigate('/login/created')
        }).catch((err) => {
            alert("Failed to create account!\n" + err);
        });
    }
</script>
<fieldset>
    <legend>Register</legend>
    
    <label for="name">Name</label>
    <input type="text" id="name" name="name" /><br>
    
    <label for="email">Email *</label>
    <input type="email" id="email" name="email"><br>

    <label for="password">Password *</label>
    <input type="password" id="password" name="password"><br><br>

    <button on:click={register}>Register</button> <span>Already got an account? <Link to="/login">Log in</Link></span>
</fieldset>