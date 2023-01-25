# PizzaDashboard

View orders made with my demo pizza ordering phone system (not yet public)

Written in Svelte+Vite with Appwrite as the backend.

## Get this running
### Requirements
You'll need an Appwrite server with the following:

One DB called `pizza_shop`, containing one tables called `orders`, with the following attributes:
* `id` - int
* `status` - int
* `total` - double (aka float)
* `order` - string

You'll need to create Indexes for `id`, `$id` and `status`.

Permissions on the table should be:

* Any - Read
* Users - Read, Update
* `team:create` - Create, Read, Update, Delete

Create a team called `create`. Users in this team can delete orders. In the future they might also be able to create orders.

I plan to put up the DB file at some point.

### Steps

1. Create a `.env` file in the root:
```
APPWRITE_ENDPOINT=...
APPWRITE_PROJECT=...
```

2. Install dependancies - `npm install`
3. Run - `npm run dev`, or build - `npm run build`