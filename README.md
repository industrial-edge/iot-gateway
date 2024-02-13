# IoT Gateway - PLC to Edge to AWS Cloud

Using the Industrial Edge system apps to preprocess PLC data and push it to the cloud

- [IoT gateway](#iot-gateway)
  - [Description](#description)
    - [Overview](#overview)
    - [General task](#general-task)
  - [Requirements](#requirements)
    - [Used components](#used-components)
    - [Further requirements](#further-requirements)
    - [TIA Project](#tia-project)
  - [Installation](#installation)
  - [Usage](#usage)
  - [Documentation](#documentation)
  - [Contribution](#contribution)
  - [Licence and Legal Information](#licence-and-legal-information)

## Description

This example demonstrates how to use the Industrial Edge System Apps to read data from a PLC, preprocess it and push it to the cloud.

### Overview

 <a href="docs/graphics/iot-gatewagit y-overview.png"><img src="docs/graphics/iot-gateway-overview.png" height="70%" width="70%" ></a> 
  <br>

The **OPC UA Connector** is used to read status data from the PLC in a 100ms cycle and publishes it to the **Databus**.

The **Flow Creator** is used to read the data and calculate the flow in m3/min and publish it back to the **Databus**.

The **Cloud Connector** then reads this preprocessed data and publishes it to the **AWS Cloud**.

### General task

To set up this example, the industrial Edge Apps Databus, OPC UA Connector and Cloud Connector have to be configured. 
Furthermore an AWS IoT Devices has to be created.

## Requirements

### Used components

- Industrial Edge Management (IEM) v2.0.1-1 
  - Databus Configurator V2.3.2-2
  - Common Connector Configurator V1.9.1-1
  - Cloud Connector Configurator V1.17.0-3
- Industrial Edge Virtual Device (IEVD) 1.16.1-1 
  - Databus V2.3.1-2
  - Common Import Converter V2.1.0-2
  - IIH Registry Service V1.9.0-0
  - IIH Essentials V1.10.0
  - OPC UA Connector V2.0.1-0
  - Cloud Connector V1.14.0-2
  - Flow Creator V1.17.0-2
  
- PLC: CPU 1511 FW 2.8.3

### Further requirements

- The Bottle filling example project is loaded to a PLC on which a OPC-UA server is running
- The minimum publishing interval and sampling interval of the OPC UA server of the PLC is set to 100ms
- The following Apps are installed on the Industrial Edge Device (IED): *Databus, Common Import Converter, IIH Registry Service, IIH Essentials, OPC UA Connector, Cloud Connector and Flow Creator* 
- The following Apps are installed on the Industrial Edge Management (IEM) in the *Application Manager* Platform: *Databus Configurator, Common Connector Configurator and Cloud Connector Configurator* 
- Access to AWS with permission to create IoT Devices and policies

### TIA Project

The used TIA Portal project can be found in the [miscellaneous repository](https://github.com/industrial-edge/miscellaneous) in the tank application folder and is also used for several further application examples:

- [Tia Tank Application](https://github.com/industrial-edge/miscellaneous/tree/main/tank%20application)

## Installation

Please refer to the [Installation](./docs/Installation.md) documentation.

## Usage

To publish preprocessed data of the PLC to the cloud, the Flow Creator is used to receive the data published to the Databus by the OPC UA Connector, preprocess it and publish the result to the topic the Cloud Connector is listening to.

With the following flow, the filling level of the tank is used to calculate the flowrate of the tanks filling which is then published to the AWS IoT thing.

Configure the Industrial Edge System Applications according to the [Installation](./docs/Installation.md) documentation and deploy the provided [flow](./src/iotGateway-flow.json) into the Flow creator. 

Before deploying the flow enter the user credentials configured in Databus in the MQTT-Node.

![bottlefilling example](./docs/graphics/iot-gateway-flow.png)

Start the flow and the PLC Program. The calculated flowrate is now visible in the aws IoT management console:

![aws result flowrate](./docs/graphics/iot-gateway-aws-console-flowrate.png
)

## Documentation

You can find further documentation and help in the following links:

  - [Industrial Edge Hub]( https://iehub.eu1.edge.siemens.cloud/#/documentation)
  - [Industrial Edge Forum]( https://forum.mendix.com/link/space/industrial-edge)
  - [Industrial Edge landing page]( https://new.siemens.com/global/en/products/automation/topic-areas/industrial-edge/simatic-edge.html)
  - [Industrial Edge GitHub page]( https://github.com/industrial-edge)
  - [Industrial Edge documentation page]( https://docs.eu1.edge.siemens.cloud/index.html)
 
## Contribution
 
Thank you for your interest in contributing. Anybody is free to report bugs, unclear documentation, and other problems regarding this repository in the Issues section.
Additionally everybody is free to propose any changes to this repository using Pull Requests.
 
If you haven't previously signed the [Siemens Contributor License Agreement](https://cla-assistant.io/industrial-edge/) (CLA), the system will automatically prompt you to do so when you submit your Pull Request. This can be conveniently done through the CLA Assistant's online platform. Once the CLA is signed, your Pull Request will automatically be cleared and made ready for merging if all other test stages succeed.
 
## License and Legal Information
 
Please read the [Legal information](LICENSE.md).
