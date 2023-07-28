---
weight: 1
title: Get started
pcx_content_type: get-started
---

# Get started

This guide will instruct you through:

- Creating a KV namespace.
- Interacting with your KV namespace.
- Using environments with KV namespaces.

## Prerequisites

To use Workers KV, you will need:

1. A [Cloudflare account](/fundamentals/account-and-billing/account-setup/), if you do not have one already. 
2. [Wrangler](/workers/wrangler/install-and-update/) installed.

## 1. Create a KV namespace 

A KV namespace is a key-value database that is replicated to Cloudflare’s global network.

You can create a KV namespace via Wrangler or the Cloudflare dashboard.

### Create a KV namespace via Wrangler

Wrangler allows you to put, list, get, and delete entries within your KV namespace.

To use Workers KV, you must create a KV namespace. KV operations are scoped to your account.

To create a KV namespace via Wrangler:

1. Run `wrangler kv:namespace create <YOUR_NAMESPACE>` in your terminal.

The `kv:namespace` subcommand takes a new binding name as its argument. A Workers KV namespace will be created using a concatenation of your Worker’s name (from your `wrangler.toml` file) and the binding name you provide:

```sh
$ wrangler kv:namespace create <YOUR_NAMESPACE>
🌀  Creating namespace with title <YOUR_WORKER-YOUR_NAMESPACE>
✨  Success!
Add the following to your configuration file:
kv_namespaces = [
  { binding = <YOUR_BINDING>, id = "e29b263ab50e42ce9b637fa8370175e8" }
]
```

2. In your `wrangler.toml` file, add the following with the values generated in your terminal:

```bash
---
filename: wrangler.toml
---
kv_namespaces = [
    { binding = "<YOUR_BINDING>", id = "<YOUR_ID>" }
]
```

Binding names do not need to correspond to the namespace you created. It is an entirely new value that you assign.

{{<Aside type="note" header="Bindings">}}

A binding is a how your Worker interacts with external resources such as [KV Namespaces](/kv/learning/kv-namespaces/), [Durable Objects](/durable-objects/), or [R2 Buckets](/r2/api/workers/workers-api-reference/). A binding is a runtime variable that the Workers runtime provides to your code. You can declare a variable name in your `wrangler.toml` file that will be bound to these resources at runtime, and interact with them through this variable. Every binding's variable name and behavior is determined by you when deploying the Worker. 

Refer to the [Environment Variables](/workers/platform/environment-variables) documentation for more information.

{{</Aside>}}

### Create a KV namespace via the dashboard

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com).
2. Select **Workers & Pages** > **KV**.
3. Select **Create a namespace**. 
4. Enter a name for your namespace. 
5. Select **Add**.

## 2. Create an access token

You only need an access token if you do not use the bindings directly. 

To create an access token:

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com).
2. Select **My Profile** > **API Tokens**.
3. Select **Create Token** > **Edit Cloudflare Workers** > **Use template**.
4. Under **Permissions**, select **Account** > **Workers KV Storage** > **Edit**.
5. Select **Continue to summary**.

## 3. Interact with your KV namespace

To write a value to your empty KV namespace using Wrangler, run the `wrangler kv:key put` subcommand in your terminal, and input your key and value respectively:

```sh
$ wrangler kv:key put --binding=<YOUR_BINDING> "<KEY>" "<VALUE>"
Writing the value "<VALUE>" to key "<KEY>" on namespace e29b263ab50e42ce9b637fa8370175e8.
```

You can now access the binding from within a Worker. In your Worker script, use the KV `get()` method to fetch the data you stored in your KV database:

```js
let value = await <YOUR_BINDING>.get("KEY");
```

Instead of using `--binding`, you may use `--namespace-id` to specify which KV namespace should receive the operation:

```sh
$ wrangler kv:key put --namespace-id=e29b263ab50e42ce9b637fa8370175e8 "<KEY>" "<VALUE>"
Writing the value "<VALUE>" to key "<KEY>" on namespace e29b263ab50e42ce9b637fa8370175e8.
```

A KV namespace can be specified in two ways:

1.  With a `--binding`:

    ```sh
    $ wrangler kv:key get --binding=<YOUR_BINDING> "<KEY>"
    ```

  This can be combined with `--preview` flag to interact with a preview namespace instead of a production namespace.

2.  With a `--namespace-id`:

    ```sh
    $ wrangler kv:key get --namespace-id=<YOUR_ID> "<KEY>"
    ```

Refer to the [`kv:bulk`](/kv/platform/kv-commands/#kvbulk) documentation to write a file of multiple key-value pairs to a given KV namespace.



## Related resources

* [Environments](/workers/platform/environments/)
* [`kv` command documentation](/kv/platform/kv-commands/)
* [`wrangler.toml` configuration documentation](/workers/wrangler/configuration)