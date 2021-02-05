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

To run this, you will need a Node.js runtime (local, container, Cloud Foundry, for example), bound to an IBM Internet of Things Foundation (WIOTF) service instance.
*NOTE:* you will need to modify the `manifest.yaml` to refer to your WIOTF service name. 

The application will publish events using a generated deviceId name - this defaults to `mqtt_iot_` followed by a random hex number; this can be made more deterministic by setting the `MQTT_DEVICE_PREFIX` environment variable, either in the `manifest.yaml` or from the Cloud Foundry application console (Runtime --> Environment variables). If the environment variable is set, the deviceId name will be the prefix string appended with the instance index of the Cloud Foundry app; as the application is scaled up from 1, the index will rise from 0.

Once running you will be able to control the simulated obvserations through a simple web page:
![control page](/assets/control-page.png)
 
<details><summary>Example Environment settings from Cloud Foundry</summary>
<pre>
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
</pre>
</details>

## Running in the IBM Cloud

This app can be run as a Cloud Foundry node.js application, by 
1. cloning this repository locally running the [ibmcloud cli](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli) `ibmcloud cf push` command
1. cloning/forking this repository to github, creating an [open toolchain], and linking to your copy of the repository

*_In either case, you will need to update the manifest.yaml file to set the service name for your Internet of Things service instance._*

[![Deploy to IBM Cloud](https://cloud.ibm.com/devops/setup/deploy/button.png)](https://cloud.ibm.com/devops/setup/deploy?repository=https://github.com/ibmrcruicks/nodejs-iotf-iotsensor)

You can create a free Internet of Things service using [IBM Cloud catalog - IOT](https://cloud.ibm.com/catalog/services/iotf-service).
## Running locally

This app can be run locally, by
1. clone this repository locally
1. run `npm install`
1. export the required environment variables for your IOT Service
1. run `npm start`
1. browse to the [control page](http://localhost:3000)

## Actuator Command interface

A simple command interface, implemented via MQTT messaging, allows applications to send commands to the simulated device; in this example, the command is implemented as an event-type of `cmd`, with a message format of `{"set":"<css-color-name>"}`. The effect is to change the color of the `Actuator` display in the user interface:

![iotsensor-ui](/assets/iotsensor-ui.png)

Set the color using values from [W3Schools CSS Colors](https://www.w3schools.com/cssref/css_colors.asp).

## Timeseries injection

Using a similar mechanism to the Actuator interface, the sensor can be told to adjust its temperature setting, using an event-type of `timeseries` with a message format of `{"set":int-or-float-number}`. The effect is to change the current base set value, and the associated observed value. 

Use an external MQTT application to inject the timeseries values via generation, or from a sample dataset.

A sample [Node-RED flow](/assets/command-flow.json) shows how to manually send commands, and inject timeseries value changes.

![node-red-command](/assets/command-flow.png)

