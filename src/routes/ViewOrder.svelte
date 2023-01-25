<script>
    import { Databases } from "appwrite";
    import Login from "./Accounts/Login.svelte";
    import { appwrite, account } from "./Appwrite.svelte";
    import { parseItem, orderStatus } from "../Items.svelte";
    import { Link } from "svelte-routing";
    
    export let orderid;
    
    const database = new Databases(appwrite);
    
    async function getOrder(orderid) {
        let order = await database.getDocument('pizza_shop', 'orders', orderid).catch()
        
        order.order = JSON.parse(order.order.replace(/\\/g,''))
        
        console.log(order)
        
        return order
    }
</script>

{#await getOrder(orderid) then order}
    <h1>Order {orderid}</h1>

    {#await account.get() then account}
        <Link to="/"><button>Back to list</button></Link>
    {/await}

    <h3>Order details</h3>
    
    {#each order.order as item}
        {parseItem(item)} - £{item.price}<br>
    {/each}

    <br>
    <b>Order total - £{order.total}</b><br><br>

    <span>Order status: {orderStatus[order.status]}</span>
{:catch}
    <h1>Order not found!</h1>
{/await}