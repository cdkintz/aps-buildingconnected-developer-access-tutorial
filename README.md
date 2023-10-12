# Getting Started with the BuildingConnected API

This tutorial explains all the steps required to make your first successful API call. It covers authentication using Autodesk Platform Services’ (APS) authentication API. It also explains how to use the BuildingConnected developer experience in the event you don’t have a BuildingConnected account.

## Prerequisites
1. You must first create an application on APS ([find tutorial here](#))
2. You must have a BuildingConnected account
3. Your BuildingConnected user must be linked with an Autodesk ID ([find tutorial here](#))
4. You must have a relevant BuildingConnected subscription for each endpoint
    1. Users (no subscription required)
    2. Opportunities (Bid Board Pro)
    3. All others (BuildingConnected Pro)

## Authentication

Authenticate using APS ([find documentation here](#)). BuildingConnected only accepts requests using 3-legged authentication (“3LO”). It is a common misconception that 3LO requires users to manually authorize each request. APS provides “refresh tokens” that enable an application to remain persistently authenticated into perpetuity. Refresh tokens only expire after 14 days. They can be exchanged for a new refresh token at any time during that period, giving your application the ability to refresh its authentication without requiring a user to log in.

Follow these steps to create an application that can run unmanned programs against the BuildingConnected API.

After satisfying the prerequisites above:

1. Generate an authorization code described in this tutorial (this step requires user sign-in)
2. Continue following the tutorial to exchange the authorization code for a 3-legged token, which will give you a response that includes a refresh token (e.g., `"refresh_token": "l8rZq7ckWJ2KocuJg0hqu3oSd8AzG4jsypcXzl4ZUo"`).
3. When scheduling API calls, call the `POST token` endpoint and exchange your refresh token for a new one, which will give you both a new access token and a new refresh token 

Use the token to make your first API call:

    curl -v 'https://developer.api.autodesk.com/construction/buildingconnected/v2/projects?filter[updatedAt]=2020-05-01T06:00:00.000Z..' \
      -H 'Authorization: Bearer AuIPTf4KYLTYGVnOHQ0cuolwCW2a'

Notes: 

- Pay attention to the header requirements for the /token endpoint:
    - The authorization value must be base 64 encoded as `"value = encode(client_id:client_secret)"`
    - Content-type must be `application/x-www-form-urlencoded`
- If you are creating a direct PowerBI application, you may need to use PKCE authentication ([see tutorial](#)) but we recommend all our clients to set up an ETL ([see tutorial](#)) so they maximize access to their data

## Accessing the developer environment

If you are a developer without a BuildingConnected account, you can use the developer environment to mock API calls. The only requirement is that you have created an application on APS and enabled the BuildingConnected API. 

There are two relevant non-production environments for developers:

- `test` (for developers)
- `alpha` (for partners)

To make an API call to a non-production environment, include the headers “x-bc-mode: test” for the test environment and `x-ads-target-server: alpha` for the partner environment.

### Example Call for Developers without a BuildingConnected Account

Here is an example of a call for a developer who does not have a BuildingConnected account:

```shell
curl 'https://developer.api.autodesk.com/construction/buildingconnected/v2/opportunities' \
     -H 'Authorization: Bearer nFRJxzCD8OOUr7hzBwbr06D76zAT' \
     -H 'x-ads-target-server: test'
```

The test environment provides pre-set data to reflect what BuildingConnected customers would typically fill in the application. Here is an example:

```shell

{
    "results": [
        {
            "id": "646e3f2cf03239f2cd095b51",
            "name": "Epoxy Flooring",
            "number": "7112",
            "client": {
                "company": {
                    "id": "644a7f48f03239042f7d60fd",
                    "name": "Zemlak-Braun"
                },
                "lead": {
                    "id": "644a7f6af0323900a4d19287",
                    "email": "atidcombe1@java.com",
                    "firstName": "Alfie",
                    "lastName": "Gingle",
                    "phoneNumber": "478-976-9999"
                },
                "office": {
                    "id": "644a7f5af0323900449b6415",
                    "location": {
                        "country": "USA",
                        "state": "DC",
                        "streetName": "Schurz",
                        "streetNumber": "98",
                        "suite": "15",
                        "city": "Bridgeport",
                        "zip": "77085",
                        "complete": "220 Jackson, Bridgeport, CT 06673 USA",
                        "coords": {
                            "lat": 29.6218,
                            "lng": -73.3637
                        }
                    },
                    "name": "Texas"
                }
            },
            "competitors": [
                {
                    "bidAmount": 189802,
                    "companyId": "644a7f47f03239020137c02d",
                    "isWinner": false,
                    "name": "Tremblay, Hauck and Koch"
                },
                {
                    "bidAmount": 79900,
                    "companyId": "644a7f47f0323900a4d19282",
                    "isWinner": true,
                    "name": "Lakin-Grant"
                }
            ],
            "customTags": [
                "tag2",
                "tag3"
            ],
            "createdAt": "2022-10-06T15:18:21.000Z",
            "updatedAt": "2023-12-07T15:18:23.000Z",
            "defaultCurrency": "USD",
            "source": "BUILDINGCONNECTED",
            "isNdaRequired": true,
            "isPublic": false,
            "outcome": {
                "state": "WON",
                "otherReason": null,
                "updatedAt": "2024-02-28T15:18:51.000Z",
                "updatedBy": "644a7e29f032390779646a9e"
            },
            "requestType": "PROPOSAL",
            "submissionState": "SUBMITTED",
            "workflowBucket": "UNDECIDED_ACTIVE_PARENT",
            "isParent": true,
            "parentId": null,
            "groupChildren": [
                "646e3f32f03239f2cd095b53",
                "646e3f32f03239f2cd095b55"
            ],
            "bid": {
                "id": "646e3f30f03239f2cd095b52",
                "submittedAt": "2022-05-02T03:01:00.000Z",
                "total": 1837,
                "revision": 0,
                "type": "SEND"
            },
            "members": [
                {
                    "viewedAt": "2022-04-25T20:06:50.000Z",
                    "userId": "644a7e27f0323900449b6412",
                    "type": "ASSIGNEE"
                },
                {
                    "viewedAt": "2021-01-23T20:20:16.000Z",
                    "userId": "644a7e27f03239056f6bb624",
                    "type": "ASSIGNEE"
                }
            ],
            "dueAt": "2024-01-30T15:19:11.000Z",
            "jobWalkAt": "2022-02-04T15:18:22.000Z",
            "rfisDueAt": "2022-10-23T15:18:44.000Z",
            "expectedStartAt": "2022-09-07T15:18:57.000Z",
            "expectedFinishAt": "2023-10-09T15:19:43.000Z",
            "invitedAt": "2022-01-26T15:18:57.000Z",
            "tradeName": "Laborer",
            "projectSize": 1740,
            "projectInformation": "<div> Info2 </div>",
            "location": {
                "country": "USA",
                "state": "CT",
                "streetName": "Stang",
                "streetNumber": "960",
                "suite": "6",
                "city": "Bridgeport",
                "zip": "06606",
                "complete": "960 Stang, Bridgeport, CT 06606 USA",
                "coords": {
                    "lat": 41.2091,
                    "lng": -73.2086
                }
            },
            "tradeSpecificInstructions": "Instruction",
            "architect": "Ingrim Clarkson",
            "engineer": "Derrik McDavitt",
            "propertyOwner": "Leslie Maddox",
            "propertyTenant": "Noland Leynton",
            "declineReasons": [
                "OTHER",
                "TOO_BUSY"
            ],
            "additionalInfo": "Information",
            "priority": "LOW",
            "marketSector": "HIGHWAY_STREET",
            "rom": 96,
            "winProbability": 55,
            "followUpAt": "2021-06-08T17:56:58.000Z",
            "contractStartAt": "2021-01-11T12:53:49.000Z",
            "contractDuration": 64,
            "averageCrewSize": 84,
            "estimatingHours": 393,
            "feePercentage": 96,
            "profitMargin": 4,
            "finalValue": 565018,
            "isArchived": false,
            "owningOfficeId": "644a7e17f03239056f6bb623",
            "isSealedBidding": false,
            "clientValues": {
                "name": "Epoxy Flooring",
                "dueAt": "2024-01-30T15:19:11.000Z",
                "jobWalkAt": "2022-02-04T15:18:22.000Z",
                "rfisDueAt": "2022-10-23T15:18:44.000Z",
                "expectedStartAt": "2022-09-07T15:18:57.000Z",
                "expectedFinishAt": "2023-10-09T15:19:43.000Z",
                "tradeName": "Laborer",
                "projectSize": 1740,
                "projectInformation": "<div> Info2 </div>",
                "location": {
                    "country": "USA",
                    "state": "CT",
                    "streetName": "Stang",
                    "streetNumber": "960",
                    "suite": "6",
                    "city": "Bridgeport",
                    "zip": "06606",
                    "complete": "960 Stang, Bridgeport, CT 06606 USA",
                    "coords": {
                        "lat": 41.2091,
                        "lng": -73.2086
                    }
                },
                "tradeSpecificInstructions": "Instruction",
                "architect": "Ingrim Clarkson"
            }
        }
    ],
    "pagination": {
        "limit": 1,
        "cursorState": "646e3f2cf03239f2cd095b51",
        "nextUrl": "/construction/buildingconnected/v2/opportunities?limit=1&cursorState=646e3f2cf03239f2cd095b51"
    }
}
```

All query parameters that are outlined in the documentation are handled by the test environment. Data is not persisted from calls made to write endpoints. Only partners are provided access to the alpha environment that supports data persistence.

# Accessing the "Alpha" environment as an Autodesk Partner

Check out our [partner portal](https://construction.autodesk.com/partner-signup/) if you are interested in accessing the `alpha` environment. There are additional benefits to becoming an Autodesk Construction Cloud integration partner beyond integration support.

## If You're Already Approved to Use Alpha, Follow These Steps to Get Started

1. **Create APS Account and Application**
    - Navigate to [APS Autodesk](https://aps.autodesk.com/) and create both an APS account and application.

2. **Create BuildingConnected Company in Alpha Environment**
    - Go to [Alpha BuildingConnected](https://app-alpha.buildingconnected.com/) and click on "need account".

3. **Choose Company Option**
    - Choose "create a new company" unless one of your colleagues has already created one.

4. **Email Autodesk Partner Team**
    - Email your contact at the Autodesk partner team with the email address you used to create your BuildingConnected account.



# Use Postman to Make Your First API Call

You don’t need to know anything about code to use the API using Postman. This guide explains how to use the BuildingConnected Postman collection to use the BuildingConnected API.

1. Download the Postman files from this GitHub repository: [https://github.com/autodesk-platform-services/aps-buildingconnected.api-postman.collection](https://github.com/autodesk-platform-services/aps-buildingconnected.api-postman.collection)

2. Go to [https://identity.getpostman.com/signup](https://identity.getpostman.com/signup) and create a free account.

3. In your Postman workspace, click on the import button.

   ![image](https://github.com/cdkintz/aps-buildingconnected-developer-access-tutorial/assets/16196853/d337fcf2-4e2f-4324-9873-d0c1de854d1e)

4. Upload the Postman file.

5. In your new collection, go to Variables and add in your client id and client secret and hit save.

   ![image](https://github.com/cdkintz/aps-buildingconnected-developer-access-tutorial/assets/16196853/50665d45-7233-4845-863f-076249f08e8e)

6. Go to Authorization and click “Get New Access Token” at the very bottom.

7. You should be prompted to sign in with your Autodesk ID.

8. Once signed in, click “Use Token”.

9. Open any API call from the left bar.

10. If accessing the developer environment (if you don’t currently have a BuildingConnected paid subscription), go to Pre-request scripts and add “pm.request.headers.add("x-bc-mode: test")” and hit save.

    ![image](https://github.com/cdkintz/aps-buildingconnected-developer-access-tutorial/assets/16196853/301719aa-d741-4c83-898c-e7e8367380be)


You are free to use this Postman library for all your API calls to the BuildingConnected API. We will keep this GitHub repository up to date as we add new endpoints.



