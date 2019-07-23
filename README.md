# azure-nodejs-sdk-websitemanagement-issue
There is an issue with the documentation or the API surface of the WebsiteManagement class of the Azure NodeJs SDK

- **Package Name**: @azure/arm-appservice
- **Package Version**: 5.8.0
- **Operating system**: Windows 10
- [x] **nodejs**
  - **version**: 11.3.0
- [ ] **browser**
  - **name/version**:
- [x] **typescript**
  - **version**: 3.5.3
- Is the bug related to **documentation** in
  - [ ] README.md
  - [x] source code documentation
  - [ ] SDK API docs on https://docs.microsoft.com

**Describe the bug**
I wanted to create a CLI app with NodeJs to connect to the Azure Subscription and check if the particular AppService resource, CosmosDb resource, SqlDb resource, ApiManagement resource, etc. is exists or not.

I followed the guidelines on the below links (*they're almost identical*) to create such NodeJs app with Typescript.

- [GitHub Azure Repository](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/appservice/arm-appservice)
- [Npm Repository](https://www.npmjs.com/package/@azure/arm-appservice)
- [GitHub Azure NodeJs SDK](https://azure.github.io/azure-sdk-for-js/arm-appservice/index.html)

After the very first step, I'm having an issue on [WebSiteManagementClient](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/appservice/arm-appservice/src/webSiteManagementClient.ts) class, which is blocking me to compile the project with Typescript.

**To Reproduce**
Steps to reproduce the behavior:

1. Open new Powershell window and create new npm project with executing the following command

```bash
npm init --force
```

2. Add [@azure/arm-appservice](https://www.npmjs.com/package/@azure/arm-appservice) and [@azure/ms-rest-nodeauth](https://www.npmjs.com/package/@azure/ms-rest-nodeauth) dependencies by executing the following command

```bash
npm i @azure/arm-appservice @azure/ms-rest-nodeauth
```

3. Create a *Typescript* file (for example, *test.ts*) and paste the following code in it

```javascript
import * as msRest from "@azure/ms-rest-js";
import * as msRestAzure from "@azure/ms-rest-azure-js";
import * as msRestNodeAuth from "@azure/ms-rest-nodeauth";
import { WebSiteManagementClient, WebSiteManagementModels, WebSiteManagementMappers } from "@azure/arm-appservice";
const subscriptionId = process.env["AZURE_SUBSCRIPTION_ID"];

msRestNodeAuth.interactiveLogin().then((creds) => {
  const client = new WebSiteManagementClient(creds, subscriptionId);
  client.appServiceCertificateOrders.list().then((result) => {
    console.log("The result is:");
    console.log(result);
  });
}).catch((err) => {
  console.error(err);
});
```

**Expected behavior**
It should be compiled and worked.

But it seems, `WebSiteManagementClient` class requires `ServiceClientCredentials` in the *constructor* but the `creds` variable we're passing is type of `DeviceTokenCredentials`. So, it's not be able to compile at all.

Either documentation or the API surface of the `WebSiteManagementClient` class should be fixed.

**Screenshots**
![Screenshot 0](https://raw.githubusercontent.com/polatengin/azure-nodejs-sdk-websitemanagement-issue/master/screenshot-0.png "Screenshot 0")
![Screenshot 1](https://raw.githubusercontent.com/polatengin/azure-nodejs-sdk-websitemanagement-issue/master/screenshot-1.png "Screenshot 1")

**Additional context**
Add any other context about the problem here.
