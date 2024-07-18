# Prepare zip archive for MS Teams application

1. Create a folder with an application name
2. Put on the folder (example file attached to the task):
    1. **color.png** file which contains program icon file (280x280pt default). This icon will be shown on the MS Teams tab panel;
    2. **outline.png** file which contains the color that will be considered transparent on the icon. Usually a 32x32 pixel picture with a white fill;
    3. **manifest.json** file with an application tab presence.
3. If the application is a new one it needs registration and gets a unique id - [app registration](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade).
4. Full manifest file description available on [ms teams developers documentation](https://docs.microsoft.com/en-us/microsoftteams/platform/resources/schema/manifest-schema).
5. Minimum content required on manifest.json file:

```json
{
    "$schema": "https://developer.microsoft.com/en-us/json-schemas/teams/v1.9/MicrosoftTeams.schema.json",
    "manifestVersion": "1.9",
    "version": "application version",
    "id": "unique app id",
    "packageName": "com.microsoft.teams.extension",
    "developer": {
        "name": "WebTrit",
        "websiteUrl": "https://www.portaone.com/",
        "privacyUrl": "https://www.portaone.com/privacy/portaphone-ms-teams",
        "termsOfUseUrl": "https://www.portaone.com/terms-of-use"
    },
    "icons": {
        "color": "color.png",
        "outline": "outline.png"
    },
    "name": {
        "short": "PortaPhone",
        "full": "PortaPhone Web for MS Teams"
    },
    "description": {
        "short": "Short description",
        "full": "Full description"
    },
    "accentColor": "#FFFFFF",
    "staticTabs": [
        {
            "entityId": "index",
            "name": "Dialer",
            "contentUrl": "URL to web resource",
            "websiteUrl": "URL to web resource",
            "scopes": [
                "personal"
            ]
        },
        {
            "entityId": "about",
            "scopes": [
                "personal"
            ]
        }
    ],
    "permissions": [
        "identity",
        "messageTeamMembers"
    ],
    "devicePermissions": [
        "media"
    ],
    "validDomains": [
        "valid domen as same as in URL"
    ]
}
```

manifest.json fields to be defined:

| **field** | **description** | **example** |
| ---| ---| --- |
| version | application(tab) version | 1.0.0 |
| id | unique application identificator | a922a130-a8ee-4c6c-9263-ed9800e50e9e |
| name | section contain short and full application name | "short": "PortaPhone",<br>"full": "PortaPhone Web for MS Teams" |
| description | section contain short and full application description | "short": "Connect to your PortaSwitch CloudPBX environment from MS Teams.",<br>"full": "PortaPhone MS Teams allows users of a cloud PBX system (based on PortaSwitch) to connect to it from MS Teams client and make calls to colleagues, external numbers (landline/mobile) and receive incoming calls. A standard set of PBX features such as call transfer, call recording, and corporate directory (list of cloud PBX extensions) are supported. This application uses a demo PortaSwitch environment, operated by PortaOne - contact PortaOne sales team to obtain access credentials. You will log in using your phone number and a one-time password (OTP) will be delivered to your registered email address."<br> |
| staticTabs | the section describes the resource that will open when you click on the tab | "entityId": "index",<br>"name": "Dialer",<br>"contentUrl": "[https://dialer.demo.portaone.com/](https://dialer.demo.portaone.com/)", |

6\. After editing manifest.json file folder content will be zipped. Only files without folders. Zip file name not regulated, it is desirable to indicate the name of the application and its version.

  

**Solution**: for disable navigation drawer can minimize need to pass special parameters in URL. See part of manifest json:

```json
    "staticTabs": [
        {
            "entityId": "index",
            "name": "Dialer",
            "contentUrl": "dailer.com/home?mode=mst",
            "websiteUrl": "dailer.com/home?mode=mst",
            "scopes": [
                "personal"
            ]
        },
        {
            "entityId": "about",
            "scopes": [
                "personal"
            ]
        }
    ],
```
