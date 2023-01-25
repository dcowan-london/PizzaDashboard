<script>
    import { navigate } from 'svelte-routing';
    import { account, appwrite } from './Appwrite.svelte';
    // import appwrite from './Appwrite.svelte';
    import { Databases } from 'appwrite';

    import App from "../App.svelte";
    
    import Login from './Accounts/Login.svelte';
    import Orders from "./Orders.svelte";



    export const accountDetails = account.get();

    export const database = new Databases(appwrite);
</script>

<Orders {database} {accountDetails} />
