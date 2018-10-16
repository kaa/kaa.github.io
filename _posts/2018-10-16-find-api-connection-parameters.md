---
layout: post
title: "Get API Connection parameters using the Azure REST API"
description: "Retrieve description of parameters required for ARM deployment of API Connection resources using the Azure REST API"
category: 
tags: []
---

Sometimes the documentation provided by Microsoft for Azure leaves you wondering and wanting. Here's how you can use the REST API of Azure to interrogate for details about connection parameters required to do ARM deployment of API connections.

Unless you've alread done it,

    az login

then get the access token we're going to use soon,

    az account get-access-token --output json

you'll get something like,

    {
      "accessToken": "eyJ0eXAiOi...",
      "expiresOn": "2018-10-16 16:57:22.131352",
      "subscription": "deadbeef-dead...",
      "tenant": "deadbeef-dead....",
      "tokenType": "Bearer"
    }

Make a note of the `accessToken` and the `subscription` values, and plug then into the following,

    curl -H "Authorization: Bearer $TOKEN" https://management.azure.com/subscriptions/$SUBSCRIPTION/providers/Microsoft.Web/locations/$LOCATION/managedApis/$API?api-version=2016-06-01

Where you replace $TOKEN and $SUBSCRIPTION with the values retrieved above, $LOCATION with the azure region (eg. `westeurope`) and $API with the API you wish to interrogate (eg. `zendesk`)

Example request and response,

    curl -H "Authorization: Bearer eyJ0eXAiOi..." https://management.azure.com/subscriptions/deadbeef-dead.../providers/Microsoft.Web/locations/westeurope/managedApis/zendesk?api-version=2016-06-01

and response,

    {
      "properties": {
        "name": "zendesk",
        "connectionParameters": {
          "token:SubDomain": {
            "type": "string",
            "uiDefinition": {
              ...
            }
          },
          "token": {
            "type": "oauthSetting",
            "oAuthSettings": {
              ...
            },
            "uiDefinition": {
              ...
            }
          }
        },
        "metadata": {
          "source": "marketplace",
          "brandColor": "#03363d"
        },
        "runtimeUrls": [
          "https://logic-apis-westeurope.azure-apim.net/apim/zendesk"
        ],
        "generalInformation": {
          "iconUrl":   "https://connectoricons-prod.azureedge.net/zendesk/icon_1.0.1008.  1183.png",
          "displayName": "Zendesk",
          "description": ...,
          "releaseTag": "Preview",
          "tier": "Premium"
        },
        "capabilities": [
          "actions",
          "tabular"
        ]
      },
      ...
    }