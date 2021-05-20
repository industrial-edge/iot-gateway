# IoT gateway

Using the IE system apps to preprocess PLC data and push it to the cloud

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

This example demonstrates how to use the Industrial Edge System Apps to read data from a PLC, preprocess it and to push  it to the cloud

### Overview

 <a href="docs/graphics/iot-gateway-overview.png"><img src="docs/graphics/iot-gateway-overview.png" height="70%" width="70%" ></a> 
  <br>

The SIMATIC S7 Connector is used to read status data from the PLC in a 100ms cycle and publishes it to the IE Databus
The IE Flow Creator is used to read the data and calculate the flow in m3/min and publish it back to the IE Databus
The IE Cloud Connector then reads this preprocessed data and publishes it to the AWS cloud.

### General task

To set up this example, the IE Databus, the SIMATIC S7 Connector and the IE Cloud Connector have to be configured.

## Requirements

### Used components

- SIMATIC S7 Connector V 1.0.22
- SIMATIC S7 Connector Configurator V 1.0.9
- IE Databus V 1.0.11
- IE Databus Configurator V 1.0.44
- IE Cloud Connector V 1.0.15
- IE Cloud Connector Configurator V 1.0.9
- IE Flow Creator V 1.0.3
- Industrial Edge Device V 1.0.0-34
- Industrial Edge Management System V 1.0.11
- PLC: CPU 1511 FW 2.8.3

### Further requirements

- The Bottle filling example project is loaded to a PLC on which a OPCUA server is running.
- The minimum publishing interval and sampling interval of the OPC UA server of the PLC is set to 100ms
- SIMATIC S7 Connector is installed on the industrial edge device (IED)
- IE Cloud Connector is installed on the IED
- IE Flow Creator is installed on the IED
- Access to AWS with permission to create IoT Devices and policies

### TIA Project

The used TIA Portal project can be found in the [miscellenous repository](https://github.com/industrial-edge/miscellaneous/tree/main/tank%20application) under the following name and is also used for several further application examples:

- [tia-tank-application.7z](https://github.com/industrial-edge/miscellaneous/blob/main/tank%20application/tia-tank-application.7z)

## Installation

Please refer to the [Installation](./docs/Installation.md) documentation

## Usage

To publish preprocessed data of the PLC to the cloud, the IE Flow Creator is used to receive the data published to the databus by the SIMATIC S7 Connector, prepocess it and publish the result to the topic the IE Cloud Connector is listening to.

In the bottlefilling and the following flow, the filling level of the tank is used to calculate the flowrate of the tank filling wich is then published to the aws IoT thing.
Configure the IE System Applications according to the [Installation](./docs/Installation.md) documentation and deploy the provided [flow](./src/iotGateway-flow.json) into the IE Flow creator. Before deploying the flow enter the user credentials configured in IE Databus in the MQTT-Node.

![bottlefilling example](./docs/graphics/iot-gateway-flow.png)

Start the flow and the PLC Programm. The calculated flowrate is now visible in the aws IoT management console:

![aws result flowrate](./docs/graphics/iot-gateway-aws-console-flowrate.png
)
You can find the flow [here](./src/iotGateway-flow.json)

## Documentation
  
- You can find further documentation and help in the following links
  - [Industrial Edge Hub](https://iehub.eu1.edge.siemens.cloud/#/documentation)
  - [Industrial Edge Forum](https://www.siemens.com/industrial-edge-forum)
  - [Industrial Edge landing page](https://new.siemens.com/global/en/products/automation/topic-areas/industrial-edge/simatic-edge.html)
  
## Contribution

Thanks for your interest in contributing. Anybody is free to report bugs, unclear documenation, and other problems regarding this repository in the Issues section or, even better, is free to propose any changes to this repository using Merge Requests.

## Licence and Legal Information

Please read the [Legal information](LICENSE.md).
