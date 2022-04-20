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

## Useful commands

List supported runtimes for an OS-type in App Service

``az webapp list-runtimes --os-type linux``
