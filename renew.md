## Azure IoT Edge is made up of three components:

- IoT Edge modules are containers that run Azure services, third-party services, or your own code. Modules are deployed to IoT Edge devices and execute locally on those devices.
- The IoT Edge runtime runs on each IoT Edge device and manages the modules deployed to each device.
- A cloud-based interface enables you to remotely monitor and manage IoT Edge devices.

## IOT Edge Runtime

![IOT Edge Runtime diagram](https://github.com/codess-aus/AZ-220/blob/main/assets/m06-l01-iot-edge-runtime-introduction-fc026798.png)

The IoT Edge runtime is a collection of programs that turn a device into an IoT Edge device. 
Collectively, the IoT Edge runtime components enable IoT Edge devices to receive code to run at the edge and communicate the results.

The IoT Edge runtime is responsible for the following functions on IoT Edge devices:

- Install and update workloads on the device.
- Maintain Azure IoT Edge security standards on the device.
- Ensure that IoT Edge modules are always running.
- Report module health to the cloud for remote monitoring.
- Manage communication between downstream devices and IoT Edge devices.
- Manage communication between modules on the IoT Edge device.
- Manage communication between the IoT Edge device and the cloud.
- Manage communication between IoT Edge devices.

These responsibilities can be grouped into two categories, communication and module management, which are performed by two corresponding components of the IoT Edge runtime. 
The **IoT Edge hub** is responsible for **communication**, 
The **IoT Edge agent** **deploys and monitors** the modules.

Both the IoT Edge hub and the IoT Edge agent are modules, just like any other module running on an IoT Edge device. They're sometimes referred to as the runtime modules.

*IoT Edge hub supports clients that connect using MQTT or AMQP. It does not support clients that use HTTP*

IoT Edge hub forwards authentication requests to IoT Hub when a device first tries to connect. After the first connection is established, security information is cached locally by IoT Edge hub. Subsequent connections from that device are allowed without having to authenticate to the cloud.

To **reduce the bandwidth your IoT Edge solution uses, the IoT Edge hub optimizes how many actual connections are made to the cloud**. IoT Edge hub takes logical connections from clients like modules or downstream devices and combines them for a single physical connection to the cloud. The details of this process are transparent to the rest of the solution. Clients think they have their own connection to the cloud even though they are all being sent over the same connection.

The IoT Edge agent is the other module that makes up the Azure IoT Edge runtime. It is responsible for instantiating modules, ensuring that they continue to run, and reporting the status of the modules back to IoT Hub. This configuration data is written as a property of the IoT Edge agent module twin.

The IoT Edge security daemon starts the IoT Edge agent on device startup. The agent retrieves its module twin from IoT Hub and inspects the deployment manifest. The deployment manifest is a JSON file that declares the modules that need to be started.

## IoT Edge cloud interface

It's difficult to manage the software life cycle for millions of IoT devices that are often different makes and models or geographically scattered. Workloads are created and configured for a particular type of device, deployed to all of your devices, and monitored to catch any misbehaving devices. These activities can't be done on a per device basis and must be done at scale.

![IOT Edge Cloud Interface diagram](https://github.com/codess-aus/AZ-220/blob/main/assets/m06-l01-cloud-interface-27b30369.png)

## Azure IoT Edge modules

- A module image is a package containing the software that defines a module.
- A module instance is the specific unit of computation running the module image on an IoT Edge device. The module instance is started by the IoT Edge runtime.
- A module identity is a piece of information (including security credentials) stored in IoT Hub, that is associated to each module instance.
- A module twin is a JSON document stored in IoT Hub, that contains state information for a module instance, including metadata, configurations, and conditions.

### EdgeAgent desired properties
The module twin for the IoT Edge agent is called $edgeAgent and coordinates the communications between the IoT Edge agent running on a device and IoT Hub. The desired properties are set when applying a deployment manifest on a specific device as part of a single-device or at-scale deployment.

### EdgeHub desired properties
The module twin for the IoT Edge hub is called $edgeHub and coordinates the communications between the IoT Edge hub running on a device and IoT Hub. The desired properties are set when applying a deployment manifest on a specific device as part of a single-device or at-scale deployment.



1. Which of the following choices is a required component for an Azure IOT Edge implementation? 
- IOT Edge modules [X] 
- IOT Edge gateway 
-  Linux OS 

2. Which of the following choices describes the purpose of the IOT Edge hub? 
- The IOT Edge hub is responsible for deploying and monitoring the edge modules for the IOT Edge device
- The IOT Edge hub is responsible for managing communication between modules and with downstream devices. [X] 
- The IOT Edge hub is responsible for configuring device settings when the Edge device is configured as an Edge gateway. 

3. What is the purpose of the IOT Edge agent? 
- The IOT Edge agent is responsible for deploying and monitoring the Edge modules for the IOT Edge device [X]
- The IOT Edge agent is responsible for managing communication between modules and with downstream devices.
- The IOT Edge agent is responsible for configuring device settings when the Edge device is configured as an Edge gateway. 

There are three patterns for using an IoT Edge device as a gateway: transparent, protocol translation, and identity translation:

**Transparent** – Devices that theoretically could connect to IoT Hub can connect to a gateway device instead. The downstream devices have their own IoT Hub identities and are using any of the MQTT, AMQP, or HTTP protocols. The gateway simply passes communications between the devices and IoT Hub. The devices are unaware that they are communicating with the cloud via a gateway, and a user interacting with the devices in IoT Hub is unaware of the intermediate gateway device. Thus, the gateway is transparent.

**Protocol translation** – Also known as an **opaque gateway** pattern, devices that do not support MQTT, AMQP, or HTTP can use a gateway device to send data to IoT Hub on their behalf. The gateway understands the protocol used by the downstream devices and is the only device that has an identity in IoT Hub. All information looks like it is coming from one device, the gateway. Downstream devices must embed extra identifying information in their messages if cloud applications want to analyze the data on a per-device basis. Additionally, IoT Hub primitives like twins and methods are only available for the gateway device, not downstream devices.

**Identity translation** - Devices that cannot connect to IoT Hub directly can connect to a gateway device instead. The gateway provides IoT Hub identity and protocol translation on behalf of the downstream devices. The gateway is smart enough to understand the protocol used by the downstream devices, provide them identity, and translate IoT Hub primitives. Downstream devices appear in IoT Hub as first-class devices with twins and methods. A user can interact with the devices in IoT Hub and is unaware of the intermediate gateway device.

![Patterns](https://github.com/codess-aus/AZ-220/blob/main/assets/m06-l03-iot-edge-gateway-8836b3a3.png)
