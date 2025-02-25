---
id: typescript-sdk
title: TypeScript SDK
pagination_label: TypeScript SDK
sidebar_label: TypeScript
sidebar_position: 4
sidebar_class_name: typescriptsdk
keywords: ['tsc', 'typescript', 'sdk']
description: Learn how to use the TypeScript SDK in this guide.
slug: /tools/sdk/typescript
tags: ['SDK']
---

## Start using the TypeScript SDK

Learn how to use the TypeScript SDK in this guide. The TypeScript SDK has some pre-built code examples you can use to learn how to build tools that can interact with IdentityNow (IDN).

You can find the SDK and its examples in its GitHub repo [here](https://github.com/sailpoint-oss/typescript-sdk).

The TypeScript SDK includes the following functionality: 
- [APIs](#run-an-api-request): 
	- All [V3](/idn/api/v3) and [Beta](/idn/api/beta) APIs are available.
	- The following V2 APIs are available: 

		| Collection | Endpoint | 
		| --- | --- | 
		| Workgroups | GET Workgroup, PATCH Workgroup, DELETE Workgroup, GET Workgroups, POST Workgroups, GET Workgroup Members, POST Workgroup Members, GET Workgroup Connections, POST Bulk Delete Workgroups | 
		| Organizations | GET Organization, PATCH Organization | 
	- The following CC APIs are available: 

		| Collection | Endpoint | 
		| --- | --- | 
		| Accounts | GET Accounts, POST Remove Account | 
		| Applications | GET Applications, GET Application, POST Application, POST Update Application, POST Delete Application, GET Application Access Profiles | 
		| Connectors | GET Connectors, POST Connector, POST Delete Connector, GET Export Connector, POST Import Connector | 
		| User | POST Update User Permissions | 
		| Sources | POST Account Aggregation, GET Export Account Feed | 
		| System | POST Refresh Identities | 

- [Search](#search): You can use IDN's search. To learn more about IDN's search, refer to [Search](https://documentation.sailpoint.com/saas/help/search/index.html).

- [Transforms](#transform): You can use transforms, configurable JSON objects that define easy ways to manipulate attribute data without your needing to write any code. To learn more about IDN's transforms, refer to [Transforms](/idn/docs/transforms).

- [Pagination](#paginate-results): You can use the SDK's pre-built pagination functionality to paginate the responses to your SDK requests. To learn more about pagination, refer to [Paginating Results](/idn/api/standard-collection-parameters#paginating-results).

## Requirements

You need the following to use the TypeScript SDK:

- TypeScript. To learn how to download it and set it up, go [here](https://www.typescriptlang.org/download). 

- Your tenant name in IDN. To learn how to find it, refer to [Getting Started](/idn/api/getting-started#find-your-tenant-name). The SDK will use this tenant name to connect to your IDN instance. 

- A PAT with a client secret and ID. To learn how to create one in IDN, refer to [Personal Access Tokens](/idn/api/authentication#personal-access-tokens). The SDK will use this PAT to authenticate with the SailPoint APIs. 

## Setup

To set up your TypeScript SDK, follow these steps: 

1. [Install the SDK](#install-the-sdk)
2. [Configure the SDK](#configure-the-sdk)

### Install the SDK

1. Run the `npm init` command in your project directory and answer the questions. Doing so initializes your project and creates the "package.json" file. You will use npm to update this file with the dependencies necessary to use the SDK. 
2. Create a source folder named "src". The SDK will include the `"src/**/*"` folder path when it compiles, so your SDK file must be there. 
3. Go to the "src" folder and create a file named "index.ts" in there. You will need to compile the "index.ts" file to run the SDK. You can leave this "index.ts" file empty for now. 
4. Go to the project directory and create a file named "tsconfig.json" in there. This file will contain your compiler configuration. Copy this information into your "tsconfig.json" file: 
```typescript
{
    "compilerOptions": {
      "target": "ES2020",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
      "module": "commonjs",                                /* Specify what module code is generated. */
      "moduleResolution": "node",                       /* Specify how TypeScript looks up a file from a given module specifier. */
      "esModuleInterop": true,                             /* Omit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */
      "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */
      "strict": true,                                      /* Enable all strict type-checking options. */
      "skipLibCheck": true,
      "outDir": "./build",
      "rootDir": "src", 
      "sourceMap": true
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules"]
  }
```
5. Run the `npm install sailpoint-api-client` to install the SDK. 
6. Run the command `npm install -D ts-node` to install the TypeScript execution engine. Once ts-node is installed and you are configured, you will be able to run the `tsc src/index.ts` command to compile your project. Doing so creates a corresponding "index.js" JavaScript file you can run with this command: `node src/index.js`. 
7. [Configure the SDK](#configure-the-sdk). You will need to authenticate to connect to the SDK. Once you are configured, you can start running the examples and get started!

### Configure the SDK
To configure the SDK, create a configuration file or save your configuration as environment variables. 
You can use any of the following ways to do so: 

#### Manual Configuration

One way to create a configuration file is to create a "config.yaml" file in your project and specify the following information in it: 

```yaml
activeenvironment: example # the key that identifies the currently active environment
authtype: pat # currently only pat and pipeline are supported if the ENV VAR SAIL_AUTH_TYPE is configured to "pipeline" it will override this value
customexporttemplatespath: "" # the path to the users custom export templates file if one is provided
customsearchtemplatespath: "" # the path to the users custom search templates file if one is provided
debug: false # the debug setting  
environments: # the configured environments  
  example:
    baseurl: https://example.api.identitynow.com
    pat:
      accesstoken: example-access-token
      clientid: example-client-id
      clientsecret: example-client-secret
      expiry: example-access-token-expiry
    tenanturl: https://example.identitynow.com
```

You must specify the following information: 
- `activeenvironment`: This key identifies the current active environment the SDK is connecting to. This environment name refers to your IDN tenant name. In the example, the key is "example". You must also make sure the environment name listed under `environments` matches the `activeenvironment`. 
- `authtype`: The authentication type. Currently only "pat" and "pipeline" are supported. Configuring ENV VAR SAIL_AUTH_TYPE to "pipeline" overrides this value. In the example, the authentication type is "pat". You must also make sure the authentication type listed under the environment name "example" matches the `authtype`. 
- `baseurl` and `tenanturl`: These refer to your IDN tenant URL. 
- `clientsecret`: The PAT's client secret. 
- `clientid`: The PAT's client ID. 

Here's an example: 

```yaml
activeenvironment: devrel # the key that identifies the currently active environment
authtype: pat # currently only pat and pipeline are supported if the ENV VAR SAIL_AUTH_TYPE is configured to "pipeline" it will override this value
customexporttemplatespath: "" # the path to the users custom export templates file if one is provided
customsearchtemplatespath: "" # the path to the users custom search templates file if one is provided
debug: false # the debug setting  
environments: # the configured environments 
  example:
    baseurl: https://devrel.api.identitynow.com
    pat:
      accesstoken: example-access-token
      clientid: g0567b766b413b22c05c66e75d532f1b
      clientsecret: cabd0e950a7230b63c1ff45be33fb22065b382b6251a73c61177a8bb5482fcc7
      expiry: example-access-token-expiry
    tenanturl: https://devrel.identitynow.com
```

You can also specify this optional information: 
- `customexporttemplatespath`: Specifies the folder path to save your custom export templates file in. 
- `customsearchtemplatespath`: Specifies the folder path to save your custom search templates file in.
- `debug`: The debug setting. By default, it's set to "false".
- `accesstoken`: The PAT's name. 
- `expiry`: The PAT's expiry date.

#### CLI assisted configuration 
Another way to create a configuration file is to use the SailPoint CLI. To learn how to use the SailPoint CLI to create a configuration file, refer to [Assisted Configuration](https://github.com/sailpoint-oss/sailpoint-cli#manual-configuration).

#### Environment variable configuration 
You can also store your configuration in environment variables. 

On **Linux/Mac**, export the following environment variables:
```shell
export SAIL_BASE_URL=https://{tenant}.api.identitynow.com
export SAIL_CLIENT_ID={clientID}
export SAIL_CLIENT_SECRET={clientSecret}
```

To get your environment variables to persist across terminal sessions, add these exports to your shell profile, something like `~/.bash_profile`.
On **Windows PowerShell**, run the following commands: 
```powershell
$env:SAIL_BASE_URL=https://{tenant}.api.identitynow.com
$env:SAIL_CLIENT_ID={clientID}
$env:SAIL_CLIENT_SECRET={clientSecret}
```
To get your environment variables to persist across PowerShell sessions, use these commands instead: 
```powershell
[System.Environment]::SetEnvironmentVariable('SAIL_BASE_URL','https://{tenant}.api.identitynow.com')
[System.Environment]::SetEnvironmentVariable('SAIL_CLIENT_ID','{clientID}')
[System.Environment]::SetEnvironmentVariable('SAIL_CLIENT_SECRET','clientSecret}')
```

## Run an example request

Once your SDK is installed and configured, you can start accessing the SDK's different functionalities. The SDK includes some prebuilt examples you can copy into your PowerShell instance to start learning how to use the SDK. 

Use the examples to learn how to do the following: 
- [Transform](#transform)
- [Run an API request](#run-an-api-request)
    - [Paginate results](#paginate-results)
- [Search](#search)
  - [Paginate search results](#paginate-search-results)

### Transform

To get start using the SDK to create, manage, and delete transforms, you can copy this example request into your "index.ts" file: 
```typescript
import { Configuration, TransformsApi, TransformsApiCreateTransformRequest} from "sailpoint-api-client"

const createTransform = async () => {

    let apiConfig = new Configuration()
    let api = new TransformsApi(apiConfig)
    let transform: TransformsApiCreateTransformRequest = 
    {
        transform:
        {
            name: "Test Transform",
            type: "dateFormat",
            attributes: {
                inputFormat: "MMM dd yyyy, HH:mm:ss.SSS",
                outputFormat: "yyyy/dd/MM"
            }
        }
    }
    const val = await api.createTransform(transform)
    console.log(val)
}
createTransform()
```
This example transform, named `"Test Transform"` connects to the `TransformsApi` and uses the `TransformsApiCreateTransformRequest` endpoint to transform the `"dateFormat"` attribute so that date inputs in the `"MMM dd yyyy"` and `"HH:mm:ss.SSS"` formats are transformed into date outputs in the `"yyy/dd/MM"` format. 

To make sure that your SDK is connecting to the APIs you need, you can specify the API within the curly brackets in `import {}` at the top of the "index.ts" file. In this example, you could add `AccountsApi` and `AccountsApiCreateAccountRequest` to add the functionality to create accounts as well as transforms. 

To compile the file, first run the `tsc src/index.ts` command. This command creates a corresponding `index.js` file you can use to run the SDK. 

To run the SDK, run the `node src/index.js` command. This command sends the request and outputs a confirmation that your new transform has been created in IDN. This transform will then be available in your IDN tenant. 

### Run an API request

One of the most useful functionalities of the SDK is your ability to call the SailPoint API. 

Here's an example of the SDK using the API: 
```typescript
const getPaginatedAccounts = async () => {

    
    let apiConfig = new Configuration()
    apiConfig.retriesConfig = {
        retries: 4,
        retryDelay: axiosRetry.exponentialDelay,
        onRetry(retryCount, error, requestConfig) {
            console.log(`retrying due to request error, try number ${retryCount}`)
        },
    }
    let api = new AccountsApi(apiConfig)
    
    const val = await Paginator.paginate(api, api.listAccounts, {limit: 100}, 10)

    console.log(val)

}
```
This example calls the `AccountsApi` and uses the `listAccounts` endpoint to list the accounts in your IDN tenant. You can manipulate this example to call different APIs and create totally new tools.

To make sure that your SDK is connecting to the APIs you need, you can specify the API within the curly brackets in `import {}` at the top of the "index.ts" file. In this example, you could add `AccountsApi` and `AccountsApiCreateAccountRequest` to add the functionality to create accounts as well as transforms. 

#### Paginate results

The earlier example listing accounts in your tenant also has pagination implemented. By default, your requests will return a maximum of 250 records. To return more, you must implement pagination. To learn more about pagination, refer to [Paginating Results](/idn/api/standard-collection-parameters/#paginating-results). 

You can use the SDK to implement pagination for your API, search, and transform requests' results. 

The pagination is implemented in this line: 
```typescript
const val = await Paginator.paginate(api, api.listAccounts, {limit: 100}, 10)
```
The `limit` specifies the total number of results you can return, 100. The following unlabeled number, 10, refers to the `increment`, the number of records per page. For example, changing the `limit` to 50 and the following "10" to 5 would change the request to return a total of 50 records, 5 at a time. 

You can also provide an `initialOffset` value to specify the record number to start the request on. For example, you can provide add `{initialOffset: 11}` to start getting accounts from 11 instead of 0. 

To find out whether an endpoint supports pagination, refer to its documentation. Any API supporting pagination lists the optional query parameters detailed in [Paginating Results](/idn/api/standard-collection-parameters/#paginating-results).

### Search 

To try using the IDN [search functionality](/idn/api/v3/search-post) along with pagination, copy this code into your "index.ts" file:

```typescript
const search = async () => {
    let apiConfig = new Configuration()
    let api = new SearchApi(apiConfig)
    let search: Search = {
        indices: [
            "identities"
        ],
        query: {
            query: "*"
        },
        sort: ["-name"]
	}
    const val = await Paginator.paginateSearchApi(api, search, 100, 1000)

    for (const result of val.data) {
        const castedResult: IdentityDocument = result
        console.log(castedResult.name)
    }
    
}
```
This example returns 1000 identities, 100 at a time, and sorts them in descending order by name. You can also change the search pagination by changing "100" and "1000", respectively. 

The two main ways you can manipulate this example are to change the `indices` or the `query`. If you add `"access profiles"` to the indices, and the SDK will search access profiles too. If you change the query to "a*", the search will return all records starting with the letter "a". 

You can also change the sorting logic in the brackets next to `sort`. 

## Get started

You can use this SDK to build new tools that extend your IDN platform and improve experiences across your organization. Use this guide to get started, and if you have questions, don't hesitate to reach out on the SailPoint Developer Community forum at https://developer.sailpoint.com/discuss! 
