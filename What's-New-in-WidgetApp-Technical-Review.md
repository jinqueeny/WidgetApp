This file includes the following sections:
<!-- TOC -->
- [What's New Section for User Guide](#whats-new-section-for-user-guide)
- [What's New Section for Administrator Guide](#whats-new-section-for-administrator-guide)
    - [How do Streaming API and Streaming frontend (SFE) work?](#how-do-streaming-api-and-streaming-frontend-sfe-work)
    - [What's changed](#whats-changed)
- [What's New Section for API Guide](#whats-new-section-for-api-guide)
     - [Authentication](#authentication)
     - [Connecting to the Streaming API](#connecting-to-the-streaming-api)
     - [Working with the Streaming API](#working-with-the-streaming-api)

<!-- /TOC -->


# What's New Section for User Guide

> Internal Notes within Anonymous Ltd.: User guide is for non-technical domain experts in Things on how to use the app. 

Are you tired of working on a Thing alone in WidgetApp and not being able to collaborate with your team in real-time? Well, now you can! 
Welcome to the WidgetApp Vx.x! With our new Streaming API and the Streaming frontend (SFE), now you can work together with your team more efficiently and make sure everyone is on the same page!

![A gif recording multiple users editing the same Thing]()

With the new WidgetApp Vx.x, you can invite your team members to work on your projects with you. Don't worry, all access is securely managed behind the scenes so everyone only sees what they need to. Collaborating has never been easier!

Invite your team member to work on Things together today!

To learn more, see [WidgetApp Vx.x release notes]().

> ## My Approach
>
> I decided to focus on the user-facing aspects of the new feature, highlighting how it empowers users and adds value. By providing a recorded gif, users can see firsthand how the feature works in action. To encourage users to try the new feature, I've also included a Call To Action to inspire them to take the next step.

> ## Missing information
>
> I am interested in learning more about the use cases, examples and functionalities of the collaboration features such as sharing, commenting, co-authoring, update tracking, and access control in the new feature, and how to use them.

# What's New Section for Administrator Guide
> Internal Notes within Anonymous Ltd.: Administrator guide are for technical support staff on how to install, configure, and maintain the web server.

WidgetApp Vx.x introduces a new Streaming API and a Streaming frontend (SFE) to allow multiple users to collaborate on the same Thing at the same time. 


### How do Streaming API and Streaming frontend (SFE) work?
Streaming API runs over websockets and different services can use the same websocket connection. Most user actions, like search or scrolling, work by calling `_ajax`/ endpoints, which can be found in the [Endpoint Reference](Link-to-the-full-endpoint-reference). External systems can connect using `wss://www.widgetapp.com/_stream`.

SFE quickly propagates changes to all clients using the Streaming API. External systems that use SFE must listen on port 200 for stream messages from any `widgetApp_useraction_xxx` APIs (such as `widgetApp_useraction_refresh()`, `widgetApp_useraction_edit()`, etc.).

This feature is only available to User Groups that have been granted access using OAuth2.0 access tokens and does not affect authentication managed by the WidgetApp Frontend (WFE) in previous versions.

### What's changed

In WidgetApp Vx.x, WFE handles existing API requests as per [previous versions](Link-to-the-API-request-handling-section-in-previous-version), but hands over to the SFE for authorized connections that use the new Streaming API. SFE then manages the session in the same way as WFE, but with the added capabilities of the Streaming API.

Note that SFE and Streaming API processes add to server load. The default setup is to launch one SFE on startup, with another started in parallel for every 10 Streaming API clients.

It is recommended to monitor `_sfe` and `_stream` processes for CPU, memory, etc. to and restart any processes whose load exceeds 50%.

To restart the `_sfe` instances, run the following command: 

```
widgetapp_admin -process _sfe -n <num_instances> -start
```

**Note:** SFE restarts will terminate the `_stream` processes  automatically for any existing connections.


> ## My Approach
> I've organized the relevant administrative notes into sections for easy reference, with a specific section titled "What's Changed" to highlight any changes that system administrators and technical support teams should be aware of.
>
> ## Missing information
> - The notes do not specify if there are any limitations or restrictions on the use of Streaming API.
> - The notes do not provide information on how to troubleshoot any potential issues that may arise.

# What's New Section for API Guide
> Internal Notes within Anonymous Ltd.: API guide is for third-party developers on how to interface with WidgetApp programmatically

WidgetApp Vx.x introduces a new Streaming API and a Streaming frontend (SFE) to allow multiple users to collaborate on the same Thing at the same time. 

The WidgetApp Frontend (WFE) continues to handle existing API requests as per previous versions. SFE allows real-time collaboration and uses the new Streaming API for authorized connections.

### Authentication 

The Streaming API uses the OAuth2.0 protocol for authentication. In this release, the authentication process remains unchanged from previous versions, and are managed through the existing WFE.

### Connecting to the Streaming API

To connect to the Streaming API, you can create a new WebSocket using `wss://www.widgetapp.com/_stream`.

### Working with the Streaming API

Most user actions, like search or scrolling, work by calling `_ajax`/endpoints, which can be found in the [Endpoint Reference](Link-to-the-full-endpoint-reference).

Listen on port 200 for stream messages from any [`widgetApp_useraction_xxx` APIs](Link-to-the-full-API-reference).

> ## My Approach
> 
> I focused on providing technical details for third-party developers on the Streaming API, including authentication, the connection URL and how to work with the new Streaming API. I also included information on how external systems that use the SFE must listen on port 200 for stream messages from certain APIs and how most user actions work. 
>
> ## Missing information
> - The notes do not mention any new parameters or methods that have been added to the API to support the Streaming feature.
> - The notes do not provide any code examples or sample use cases of how the Streaming API can be integrated into an external system.
