---
layout: post
title: Can a custom event trigger a process in SAP Build Process Automation? 
---

TLDR; No.

My last interactions with SAP Build Process Automation revolve around how you can trigger process by using events. We know that when configuring a process, one of the triggers can be set to start it is an event - [Create an event to start a process](https://help.sap.com/docs/build-process-automation/sap-build-process-automation/create-event-triggers). At the moment, the UI only let's you select the events from a dropdown this is limited to some predefined business events.

By looking under the hood (Web browser, developer tools) I noticed that when setting an event trigger, there is an HTTP request made to the API. This request contains a payload with the following structure:
```json
{
    "name": "MyCustomEventName",
    "identifier": "mycustomeventname",
    "description": "",
    "header": {
        "triggerableArtifactUids": [
            "$.436i6204-0818-3t8o-q0if-h55t46k016gh"
        ],
        "sourceArtifactUid": "sap.s4.beh.businesspartner.v1.BusinessPartner.Created.v1"
    }
}
```

The `sourceArtifactUid` field is set to `sap.s4.beh.businesspartner.v1.BusinessPartner.Created.v1`. This is the event that triggers the process. I've seen this value before when dealing with the SAP S/4HANA Cloud Business Partner events.... this is the value set in the type field of the event payload. Remember that SAP has embraced the CloudEvents specification and the type field will be included in the header (context) part of the event message. This got me wondering, what would happen if I set a different value in the `sourceArtifactUid` field in the request? Will it accept it as a trigger? Let's try it.

```json
{
    "name": "MyCustomEventName",
    "identifier": "mycustomeventname",
    "description": "",
    "header": {
        "triggerableArtifactUids": [
            "$.436i6204-0818-3t8o-q0if-h55t46k016gh"
        ],
        "sourceArtifactUid": "my.custom.event.v1"
    }
}
```

Will it work? Unfortunately, no. The service validates the value set as `sourceArtifactUid`. If it doesn't match any of the predefined values, it will not accept it. This is a good thing, as it prevents you from setting a trigger that doesn't exist. That said, it would be nice to have the ability to handle custom events that can trigger processes. I imagine that the custom event will need to be follow the CloudEvents specification but that wouldn't be a big problem. It will be great to have this type of functionality as it would allow for more flexibility and automation in the platform, e.g. events from custom applications.
