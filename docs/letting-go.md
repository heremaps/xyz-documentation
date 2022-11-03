
## Converting to from Data Hub to IML

> Data Hub is currently "[In Maintenance](https://developer.here.com/documentation/product-lifecycle-policy/customer_notice/index.html)". Interactive map layers (IML) has been identified as the successor to Data Hub. For new users considering Data Hub, HERE recommends that you start with IML on the [HERE platform](https://platform.here.com).

You can migrate Data Hub spaces associated with your [developer.here.com](https://developer.here.com) accounts to the [HERE platform](https://platform.here.com) using the self-service Data Hub migration tool.

This section outlines how to migrate your Data Hub spaces to the HERE platform using the self-service Data Hub migration tool:

## Before you migrate

Before you migrate your Data Hub spaces to the HERE Platform, HERE recommends that you perform the following pre-migration steps:

- Check with your team to see if you have an existing HERE platform organization. Any number of developer.here.com accounts can migrate their resources to a single HERE platform organization.

## Sign up for a HERE platform account

As part of the migration process, you will need to either sign up for or sign in to a HERE platform account.

> #### Note
>
> If your organization has signed up for [HERE Marketplace](https://www.here.com/platform/marketplace), contact your organization admin who can invite you to join the HERE platform organization established for your company. You can also request a free trial of the HERE platform if your company does not have an organization established for it. For more information, see the [HERE platform pricing](https://developer.here.com/pricing/platform).
>
> You may need to add a credit card to your HERE platform account as part of the migration process. For example, you must have a credit card associated with your account to migrate HERE Data Hub resources.

To sign up for a HERE platform account, follow these steps:

1. Sign in to your [developer.here.com account](https://developer.here.com/login).
2. Click **Create a platform account to get started** from the yellow page banner or navigate to [https://platform.here.com/sign-up](https://platform.here.com/sign-up).
3. Follow the prompts to complete your HERE platform account creation or click **Sign in instead** to sign in to an existing HERE platform account.

## Migrate your resources

To migrate your Data Hub spaces to the HERE platform, create or select a Data Hub token, that has **full** (all spaces) 'manageSpaces' rights for your user.

### Use the following snippet to perform a dryrun:
````
curl -X 'GET' 'https://conversion-service-eu-west-1.api-gateway.ls.hereapi.com/list/dh' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyPlatformTokenOfDestinationUser' \
  -H 'Dh-Token: data-hub-short-token'
````
### Use the following snippet to start your migration:
````
curl -X 'POST' 'https://conversion-service-eu-west-1.api-gateway.ls.hereapi.com/convert/dh' \
   -H 'accept: application/json' \
   -H 'Authorization: Bearer eyPlatformTokenOfDestinationUser' \
   -H 'Dh-Token: data-hub-short-token' \
   -d ''
````
### Use the following snippet to check the status of your jobs:
````
curl -X 'GET' 'https://conversion-service-eu-west-1.api-gateway.ls.hereapi.com/convert/dh' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer eyPlatformTokenOfDestinationUser' \
  -H 'Dh-Token: data-hub-short-token'
````

## Migrate additional accounts to your HERE platform account

You can perform the self-service migration multiple times across several developer.here.com accounts to completely migrate your Data Hub spaces. For example, if you have resources associated with two developer.here.com accounts, you can perform two migrations and associate all your Data Hub spaces to a single HERE platform organization. For each additional developer.here.com account, repeat the procedure outlined in [Migrate your resources](#migrate-your-resources).

