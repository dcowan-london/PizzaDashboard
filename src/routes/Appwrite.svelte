<script context="module">
    import { Client, Account, Databases } from 'appwrite';
    
    console.log(import.meta.env)
    
    const appwrite = new Client()
    .setEndpoint(import.meta.env.VITE_APPWRITE_ENDPOINT)
    .setProject(import.meta.env.VITE_APPWRITE_PROJECT)
    
    
    const account = new Account(appwrite);
    
    export { appwrite, account }
</script>