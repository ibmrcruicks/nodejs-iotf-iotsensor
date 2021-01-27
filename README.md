# nodejs-iotf-iotsensor
sensor emulator with raw MQTT, communicating with IBM Internet of Things Foundation instance - configure via Cloud Foundry binding/connection (VCAP_SERVICES)

** Work in progress **

![mqtt](/assets/mqtt-hor-neg.png)

A simple [MQTT](https://mqtt.org) publisher that generates a JSON observation document to an IBM IOTF service instance - example below:

```
{ d: 
  { deviceId: "mqtts-iot-2345",
    temp: 28.34,
    counter: 736
   }
  timestamp: 15202020202
}
```

To run this , you will need a Node.js runtime (local, container, Cloud Foundry, for example), bound to an IBM Internet of Things Foundation service instance.

Once running you will be able to control the simulated obvserations through a simple web page:
![control page](/assets/control-page.png)

## Environment settings

```
VCAP_SERVICES : {
"iotf-service": [
    {
      "label": "iotf-service",
      "provider": null,
      "plan": "iotf-service-standard",
      "name": "Internet of Things Platform-vava",
      "tags": [
        "internet_of_things",
        "Internet of Things",
        "ibm_created",
        "ibm_dedicated_public",
        "lite",
        "ibmcloud-alias"
      ],
      "instance_name": "Internet of Things Platform-vava",
      "binding_name": null,
      "credentials": {
        "iotCredentialsIdentifier": "xxxxx",
        "mqtt_host": "xxxxx.messaging.internetofthings.ibmcloud.com",
        "mqtt_u_port": 1883,
        "mqtt_s_port": 8883,
        "http_host": "xxxxx.internetofthings.ibmcloud.com",
        "org": "xxxxx",
        "apiKey": "a-xxxxx-vavavavava",
        "apiToken": "vavavavavavavavava"
      },
      "syslog_drain_url": null,
      "volume_mounts": []
    }
  ]
```

## Running in the IBM Cloud

This app can be run as a Cloud Foundry node.js application, by 
1. cloning this repository locally running the [ibmcloud cli](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli) `ibmcloud cf push` command
1. cloning/forking this repository to github, creating an [open toolchain], and linking to your copy of the repository

In either case, you will need to update the manifest.yaml file to set the MQTT environment for connecting to your MQTT Broker.

[![Deploy to IBM Cloud](https://cloud.ibm.com/devops/setup/deploy/button.png)](https://cloud.ibm.com/devops/setup/deploy?repository=https://github.com/ibmrcruicks/nodejs-mqtt-iotsensor)

You can create a free MQTT Broker using [cloudamqp](https://cloud.ibm.com/catalog/services/cloudamqp), 
use free cloud-based brokers like [eclipse IOT](https://mqtt.eclipse.org/), or [hivemq](https://www.hivemq.com/public-mqtt-broker/)

## Running locally

This app can be run locally, by
1. clone this repository locally
1. run `npm install`
1. export the required environment variables for your MQTT Broker
1. run `npm start`
1. browse to the [control page](http://localhost:3000)
