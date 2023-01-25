<script>
    import { link, Link, navigate } from "svelte-routing";
    import { account, appwrite } from "./Appwrite.svelte";
    import { Query, Teams } from "appwrite";
    import App from "../App.svelte";

    const teams = new Teams(appwrite);

    export let accountDetails;
    export let database;
    let orders = [];
    
    const categories = ['', 'Pizza', 'Sides'];
    const varients = ['', 'Chips', 'Onion Rings'];
    const sizes = ['', 'Large', 'Small'];
    const pizzaSizes = ['', '18-inch', '12-inch', '10-inch'];

    const orderStatus = ['', 'Ordered', 'In the kitchen', 'Ready for collection', 'Complete'];
    
    database.listDocuments('pizza_shop', 'orders', [
        Query.orderDesc('id')
    ]).then((res) => {
        orders = res.documents
        
        orders.forEach(element => {
            element.order = element.order.replace(/\\/g, '')
            element.order = JSON.parse(element.order)
            console.log(element.order)
        });
    })

    teams.list().then(res => {
        res.teams.forEach(team => {
            
        });
    })

    function updateOrderStatus(orderId) {
        let order = orders.findIndex((el) => el.id === orderId);
        orders[order].status += 1;

        let updateOrderArray = {
            'status': orders[order].status
        }
        updateOrderArray = JSON.stringify(updateOrderArray);

        database.updateDocument( 'pizza_shop', "orders", orders[order].id, updateOrderArray);
    }

    function deleteOrder(orderId) {
        database.deleteDocument( "pizza_shop", "orders", orderId).then((res) => {
            database.listDocuments('pizza_shop', 'orders', [ Query.orderDesc('id') ]).then((res) => {
                orders = res.documents
                
                orders.forEach(element => {
                    element.order = element.order.replace(/\\/g, '')
                    element.order = JSON.parse(element.order)
                    console.log(element.order)
                });
            })
        }).catch(err => {
            alert(err);
        });
    }
    
    function logout() {
        account.deleteSession('current').then(() => {
            window.location.reload();
        })
    }
</script>

<title>Orders</title>

<span style="float: right;">
    {#await accountDetails then accountDetails}
        Welcome, {accountDetails.name} <button on:click={logout}>Log out</button>
    {:catch}
        Not logged in <Link to="/login">Log in</Link>
    {/await}
</span>

<h1>Orders</h1>

<table>
    <tr>
        <th>Order ID</th>
        <th>Ordered at</th>
        <th>Status</th>
        <th>Order</th>
        <th>Total</th>
        {#await accountDetails then accountDetails}
            <th>Actions</th>
        {/await}
    </tr>
    {#each orders as order}
    <tr>
        <td>{order.id}</td>
        <td>{new Date(order.$createdAt).toDateString()} {new Date(order.$createdAt).toLocaleTimeString()}</td>
        <td>{orderStatus[order.status]}
            {#await accountDetails then accountDetails}
                {#if order.status < orderStatus.length-1}
                    <button on:click={updateOrderStatus(order.id)}>Update</button>
                {/if}
            {/await}
        </td>
        <td>
            {#each order.order as json}
                {categories[json.category]},
                {#if typeof(json.variant) !== "undefined"}
                    {varients[json.variant]},
                {/if}
                {#if json.category == 1}
                    {pizzaSizes[json.size]}
                {:else}
                    {sizes[json.size]}
                {/if}
                <br>
            {/each}

            <a href="/view/{order.id}" use:link><button>View order</button></a>
        </td>
        <td>£{order.total}</td>
        {#await accountDetails then accountDetails}
            <td>
                    {#await teams.list() then teams}
                        {#each teams.teams as team}
                            {#if team.$id === "create"}
                                <button on:click={deleteOrder(order.id)}>Delete</button>
                            {/if}
                        {/each}
                        {#if teams.teams.length === 0}
                            <span>You don't have access to any actions</span>
                        {/if}
                    {/await}
            </td>
        {/await}
    </tr>
    {/each}
</table>