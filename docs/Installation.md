# Installation

- [Installation](#installation)
  - [Setting up the Databus](#setting-up-the-databus)
  - [Setting up the OPC UA Connector](#setting-up-the-opc-ua-connector)
  - [Setting up the Connection to the cloud](#setting-up-the-connection-to-the-cloud)
    - [Creating a new policy and thing for the aws IoT device](#creating-a-new-policy-and-thing-for-the-aws-iot-device)
    - [Setting up the Cloud Connector](#setting-up-the-cloud-connector)
    - [Testing the Connection to the cloud](#testing-the-connection-to-the-cloud)

## Setting up the Databus

In this example, two topics on the IE Databus are used.

The fist one is used by the OPC UA Connector to publish the data read from the PLC:
For example: `ie/d/j/simatic/v1/opcuac1/dp/r/plc1/tankLevel`

The second is used by the IE Cloud Connector to receive data which will then be published to the cloud.
For example: `CloudConnector/toAWS`

To set up a user with publish and subscribe permission, open the IE Databus Configurator for your device and set it up accordingly. In this example, a user `edge` has permission to publish and subscribe to `ie/#` and `CloudConnector/#`

![iot gateway databus setup](./graphics/iot-gateway-databus-setup.png)

## Setting up the OPC UA Connector

- Open the OPC UA Connector Configurator
- Enter the Databus User and Password in the Settings. "Bulk Publish" must be deactivated.
- Add your PLC as a data source
- Browse the tags and select  `GDB_externalSignals_tankSignals_actLevel` with a 100ms acquisition cycle.

![iot gateway S7 connector setup](./graphics/iot-gateway-s7connector-setup.png)

## Setting up the Connection to the cloud

Before starting the Cloud Connector Configurator, a new policy and a new device have to be set up in the aws IoT management console.

### Creating a new policy and thing for the aws IoT device

In the aws IoT management console:

- navigate to "Manage" -> "All devices" -> "Things"
- Click on "Create things"
- Select "Create single thing" and then click "Next"

![iot gateway create things](./graphics/iot-gateway-create-things.png)

- enter the Thing name

![iot gateway thing name](./graphics/iot-gateway-thing-name.png)

- Select "Auto-generate a new certificate (recommended)" and then click "Next"

![iot gateway auto generate](./graphics/iot-gateway-auto-generate.png)

- Click on "Create policy"
- enter the Policy name
- in the "Policy action" box choose an action one by one for `iot:Publish, iot:Connect`
- in the "Policy resource" box enter the character "*"
- in the "Policy effect" box check to "Allow"
- Click "Create"

![iot gateway create policy](./graphics/iot-gateway-create-policy.png)

- check the policy created and then click "Create thing"

![iot gateway create thing](./graphics/iot-gateway-create-thing.png)

- download the certificate and private key

![iot gateway certificate key](./graphics/iot-gateway-certificate-key.png)

The thing has been registered successfully. Keep the certificate and private key for the Cloude Connector Configuration.

### Setting up the Cloud Connector

Now that a IoT device is created in the aws cloud, the Cloud Connector can be set up.

Configure the Cloud Connector for Edge Management Data Connection

- Under Adapters -> Click Add Topic
- Check "User-defined Topic"
- Enter a HostName: `ie-databus`, Port Number: `1883`, Username: `edge`, Password: `edge`
- The Subscription Topic is the topic to which the Cloud Connector will subscribe to listen for data to publish to the cloud. E.g. `CloudConnector/#` . This means the Cloud Connector can subscribe to all topics starting with `CloudConnector/`
- Click "Add Topic" and enter a topic name, e.g `CloudConnector/toAWS` and click save. This topic must start with the name configured in the previous step.

![iot gateway adapters](./graphics/iot-gateway-adapters.png)

- Click on Add Route 
- enter a name for the route `toAWS`

![iot gateway route](./graphics/iot-gateway-route.png)

The last part is to configure the cloud clients

- Click on "Add Client"
- Enter a Name e.g `ccID` and choose `AWS` as type and click "save"
- select the certificate and private key you downloaded when creating the aws thing
- click on the edit symbol net to the client
- enter a Client ID e.g `ccID`. This will be the ID used by the connector to publish data to the MQTT broker of the aws IoT device.
- enter the hostname of the aws endpoint. You can find the address at the "Settings" section of the aws IoT console. It looks similar to this: `abcdefghi12345.iot.us-east.amazonaws.com`
- enter `8883` as Port Number
- enter Proxy settings if needed

![iot gateway client configuration](./graphics/iot-gateway-client-configuration.png)
![iot gateway client configuration advance](./graphics/iot-gateway-client-configuration-advance.png)

- Now select the route, check the box next to the bus adaptor topic `CloudConnector/toAWS` and check the box next to Cloud Connector Clients `ccID`
- click "save" and "Deploy"

![iot gateway save deploy](./graphics/iot-gateway-save-deploy.png)

Now every data published to the bus adaptor topic of the IE Databus will get published to the aws cloud.
E.g. data published to the topic `CloudConnectot/toAWS` in the IE Databus will get published to the topic `CloudConnector/` of the aws IoT thing. The publish topic to which the IE Cloud Connector will publish the data in the aws cloud is set in the "Advanced" Tab of the IE Cloud Connector Client settings, the default value is `CloudConnector/`

### Testing the Connection to the cloud

In the AWS IoT management console:

- Navigate to "Test" -> "MQTT test client"
- in "Subscribe to a topic" enter the topic the Cloud Connector is publishing to. E.g. `CloudConnector/#`
- open the Flow Creator an publish some test data to the topic the Cloud Connector is subscribing to. E.g. `CloudConnector/toAWS`
- The data will be visible in the IoT management console

![iot gateway mqtt test](./graphics/iot-gateway-mqtt-test.png)

![iot gateway flowcreator](./graphics/iot-gateway-flowcreator.png)

![iot gateway mqtt dataflow](./graphics/iot-gateway-mqtt-dataflow.png)