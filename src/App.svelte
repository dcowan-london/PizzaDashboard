<script>
  import { Router, Link, Route } from 'svelte-routing'

  import Login from './routes/Accounts/Login.svelte';
  import Register from './routes/Accounts/Register.svelte';
  import Home from './routes/Home.svelte';
  import Test from './routes/Test.svelte';
  import ViewOrder from './routes/ViewOrder.svelte';

  export let url = "";
</script>

<Router url={url}>
  <div>
    <Route path="/login" component={Login} />
    <Route path="/login/:created" component={Login} />
    <Route path="/register" component={Register} />
    <Route path="/view/:orderid" component={ViewOrder} />
    <Route path="/" component={Home} />
    <Route path="/test" component={Test} />
  </div>
</Router>