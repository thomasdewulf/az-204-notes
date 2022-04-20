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
| Shared | Free, Shared | Resources are shared with other customers. CPU quotas are allocated to each app. Resources can't scale out |
| Dedicated | Basic, Standard, Premium, PremiumV2, PremiumV3 | Runs on dedicated VM(s). Apps in same plan share same resources. The higher the tier, the more more VM instances are available |
| Isolated | Isolated (App Service Environment) | Runs on dedicated VM(s) in a dedicated vNET. Provides network isolation and compute isolation. Maximum scale out capabilities |
| Consumption | Consumption | Only for Function Apps. Scales functions dynamically based on workload |

## Deployment slots

Only available in Standard and Premium App Service Plans

## Useful commands

List supported runtimes for an OS-type in App Service

``az webapp list-runtimes --os-type linux``
