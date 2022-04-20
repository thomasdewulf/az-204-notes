# Azure App Service

- Scale up/down: scale resources like RAM and CPU (vertical scaling)
- Scale out/in: scale machine instances running the web app (horizontal scaling)

## App Service Plans

Every app runs in App Service Plan. Defines a set of compute resources. Multiple apps can run in the same plan, sharing the resources of the plan. Azure Functions can also run in an App Service Plan.

App Service Plan define:

- Region
- Number of VM instances
- Size of VM instance(s)
- Pricing tier

Pricing tier defines which App Service features are enabled and the cost.

| Compute type | Tier(s) | Info |
| -- | -- | -- |
| Shared | Free, Shared | Resources are shared with other customers. CPU quotas are allocated to each app. Resources can't scale out. Only intended for dev and test purposes. |
| Dedicated | Basic, Standard, Premium, PremiumV2, PremiumV3 | Runs on dedicated VM(s). Apps in same plan share same resources. The higher the tier, the more more VM instances are available |
| Isolated | Isolated (App Service Environment) | Runs on dedicated VM(s) in a dedicated vNET. Provides network isolation and compute isolation. Maximum scale out capabilities |
| Consumption | Consumption | Only for Function Apps. Scales functions dynamically based on workload |

### Scaling

- Free and Shared tiers can't scale. Apps receive CPU minutes on shared VMs
- Apps in same plan share same VM instances
- Deployment slots for an app run on the same VM instances
- Diagnostic logs, backups and WebJobs also use CPU cycles and memory of the VM instances

App Service plan is the scale unit of apps in App Service. App runs on all instances for which the plan is configured. With autoscale, apps are in the plan are scaled out together based on the autoscale settings.

### When to isolate apps in separate plans

- App is resource intensive
- Need for independent scaling
- App needs resource in different geographical region

## Deployment slots

Only available in Standard and Premium App Service Plans. Swapping deployment slots warms up necessary worker instances to match production scale, eliminating downtime

## Authentication and authorization

Built-in AutN an AuthZ with minimal or no code for web apps, backends and Azure Functions.

Out of the box authentication with federated identity providers. Doesn't require any particular language , SDK, ..

Runs in the same sandbox as the application code. When enabled, every incoming HTTP request passes through the authN/authZ module. For Linux and container apps, the module runs in a separate container, isolated from application code. Because it does not run in-process, no direct integration with specific language frameworks is possible.

- Authenticates user with specified provider
- Validates, stores and refreshes tokens
- Manages the authenticated session
- Injects identity information in request headers

The module is configured using app settings and run separately from the application code.

2 auth flows: without provider SDK and without provider SDK.

- Without SDK: typically used for browser apps. Presents the login to the user directly. Also called server-directed flow or server flow
- With SDK: typically used for browser-less apps (think APIs). Application (frontend) signs in user and presents token to App Service. Also called client-directed flow or client flow

Authorization can be handled in 2 ways:

- Unauthenticated requests are deferred to the backend for further handling/authZ. Auth information is passed in the headers. Provides more flexibility in handling anonymous requests.
- All unauthenticated traffic is rejected. Can redirect to sign-in if needed

## Networking

By default, apps in an App Service are accessible through public internet and can only reach internet-hosed endpoints. There are 2 main deployment types: 

- Multitenant public service hosts App Service Plans (all tiers but Isolated)
- Single tenant App Service Environment (Isolated)

### Multitenant networking features

| Inbound | Outbound |
| -- | -- |
| App- assigned address | Hybrid connections |
| Access restriction | Gateway-required vNet integration |
| Service endpoints | vNet integration |
| Private endpoints | |

### Outbound addresses

When VM family is changed, outbound addresses change as well. 

There are multiple addresses for outbound calls. These are shared by all the apps running on the same worker VM family in the App service deployment. 

## Useful commands

List supported runtimes for an OS-type in App Service

``az webapp list-runtimes --os-type linux``

List outbound IPs currently in use

```bash
az webapp show \
    --resource-group <group_name> \
    --name <app_name> \ 
    --query outboundIpAddresses \
    --output tsv
````

List all possible outbound IPs, regardless of pricing tier

```bash
az webapp show \
    --resource-group <group_name> \ 
    --name <app_name> \ 
    --query possibleOutboundIpAddresses \
    --output tsv
```
