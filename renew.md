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

Q: A developer wants to connect devices that aren't IP-enabled to an IoT hub using an IoT Edge gateway device. The developer wants each device to appear as a separate device in IoT Hub. Which IoT Edge gateway pattern should the developer use?
* Transparent
* Protocol translation
* Identity Translation

A: When you use the Identity translation gateway pattern, non-IP enabled downstream devices appear in IoT Hub as first-class devices with twins and methods.

A developer wants to connect devices that aren't IP-enabled to an IoT hub using an IoT Edge gateway device. The developer wants only the IoT Edge gateway device to have an identity in IoT Hub. Which IoT Edge gateway pattern should the developer use?
* Transparent
* Protocol translation
* Identity Translation
A: When you use the Protocol translation gateway pattern, devices that don't support MQTT, AMQP, or HTTP can use a gateway device to send data to IoT Hub on their behalf. Only the gateway device has an identity in IoT Hub.

## IoT Hub primitives
IoT Hub sees a module instance analogously to a device, in the sense that:

* It has a module twin that is distinct and isolated from the device twin and the other module twins of that device.
* It can send device-to-cloud messages.
* It can receive direct methods targeted specifically at its identity.

Currently, modules cannot receive cloud-to-device messages or use the file upload feature.

## IoT EdgeHub dev tool
The Azure IoT EdgeHub dev tool provides a local development and debug experience. The tool helps start IoT Edge modules without the IoT Edge runtime so that you can create, develop, test, run, and debug IoT Edge modules and solutions locally. You don't have to push images to a container registry and deploy them to a device for testing.

The IoT EdgeHub dev tool was designed to work in tandem with the Visual Studio and Visual Studio Code extensions, and it works with the IoT Edge dev tool. It supports inner loop development and outer loop testing, so it integrates with the DevOps tools.

## IoT Edge dev container
The Azure IoT Edge dev container is a Docker container that has all the dependencies that you need for IoT Edge development. This container makes it easy to get started with whichever language you want to develop in, including C#, Python, Node.js, and Java. All you need to install is a container engine, like Docker or Moby, to pull the container to your development machine.

## IoT Edge runtime in a container
The IoT Edge runtime in a container provides a complete runtime that takes your device connection string as an environment variable. This container enables you to test IoT Edge modules and scenarios on a system that may not support the runtime natively, like macOS. Any modules that you deploy will be started outside of the runtime container. If you want the runtime and any deployed modules to exist within the same container, consider the IoT Edge device container instead.

## IoT Edge device container
The IoT Edge device container is a complete IoT Edge device, ready to be launched on any machine with a container engine. The device container includes the IoT Edge runtime and a container engine itself. Each instance of the container is a fully functional self-provisioning IoT Edge device. The device container supports remote debugging of modules, as long as there is a network route to the module. The device container is good for quickly creating large numbers of IoT Edge devices to test at-scale scenarios or Azure Pipelines. It also supports deployment to kubernetes via helm.

[Useful link](https://learn.microsoft.com/en-gb/training/modules/examine-iot-edge-module-development/4-module-development-test-tools)

Q: IoT Edge modules share many characteristics with IoT devices. Which of the following choices accurately describes a characteristic of an IoT Edge module?
* It can send device-to-cloud messages.
* It can receive cloud-to-device messages
* It can use the file upload feature.

A: An IoT Edge module can send messages to the cloud via the IoT Edge hub.

Q: Which of the following steps is commonly performed when creating a custom Edge module?
* Use the Azure portal to create an Azure Container Registry for your Docker container images.
* Use the IoT Edge dev tool to configure your production environment.
* Use the IoT EdgeHub dev tool to install and configure the IoT Edge runtime.

A: To create a custom Edge module, you will often use the Azure portal to create an Azure Container Registry for your Docker container images.

The agent-based option of Microsoft **Defender for IoT** includes the following components:

* IoT Hub integration.
* Device agents (optional).
* Send security message SDK.
* Analytics pipeline.

Microsoft Defender for Cloud provides you the tools needed to harden your network, secure your services and make sure you're on top of your security posture.

Microsoft Defender for IoT is enabled by default when an IoT Hub resource is created

Rules in IoT Central serve as a customizable response tool that triggers on actively monitored events from connected devices.

Q: What is the purpose of a Dashboard within an Azure IoT Central solution?
A: A Dashboard is a customizable page that displays telemetry, property, and state information for selected devices.

### Phased rollout
A phased rollout is an overall process whereby an operator deploys changes to a broadening set of IoT Edge devices. The goal is to make changes gradually to reduce the risk of making wide scale breaking changes.

### Layered deployment
Layered deployments are automatic deployments that can be combined together to reduce the number of unique deployments that need to be created. Layered deployments are useful in scenarios where the same modules are reused in different combinations in many automatic deployments.

Layered deployments have the same basic components as any automatic deployment. They target devices based on tags in the device twins, and provide the same functionality around labels, metrics, and status reporting. Layered deployments also have priorities assigned to them, but instead of using the priority to determine which deployment is applied to a device, the priority determines how multiple deployments are ranked on a device. For example, if two layered deployments have a module or a route with the same name, the layered deployment with the higher priority will be applied while the lower priority is overwritten.

The system runtime modules, edgeAgent and edgeHub, are not configured as part of a layered deployment. Any IoT Edge device targeted by a layered deployment needs a standard automatic deployment applied to it first. The automatic deployment provides the base upon which layered deployments can be added.

An IoT Edge device can apply one and only one standard automatic deployment, but it can apply multiple layered automatic deployments. Any layered deployments targeting a device must have a higher priority than the automatic deployment for that device.

The **deployment manifest** is a JSON document that describes:

* The IoT Edge agent module twin, which includes three components.
  - The container image for each module that runs on the device.
  - The credentials to access private container registries that contain module images.
  - Instructions for how each module should be created and managed.
* The IoT Edge hub module twin, which includes how messages flow between modules and eventually to IoT Hub.
* Optionally, the desired properties of any other module twins

Two modules are required in every deployment manifest: $edgeAgent, and $edgeHub. These modules are part of the IoT Edge runtime that manages the IoT Edge device and the modules running on it.

IoT Edge devices can be anything from a Raspberry Pi to a laptop to a virtual machine running on a server. You may have access to the device either physically or through a virtual connection, or it may be isolated for extended periods of time. Either way, you want to make sure that it's configured to work appropriately.

Important
* Install production certificates.
* Have a device management plan.
* Use Moby as the container engine.

Helpful
* Choose upstream protocol.

The two runtime modules both have an UpstreamProtocol environment variable. The valid values for the variable are:
* MQTT
* AMQP
* MQTTWS
* AMQPWS
Configure the UpstreamProtocol variable for the IoT Edge agent in the config.yaml file on the device itself

In the high-level deployment process for IoT Edge modules, Retrieve the status of the devices after configuration.
Retrieving the status of the devices after configuration helps to verify that the module deployed successfully.

Q: What is an IoT Edge deployment manifest used for?
A: It tells your device which modules to install and how to configure them to work together.
The deployment manifest is a JSON document that tells your device which modules to install and how to configure them to work together.

## Time to live
The time to live setting is the amount of time (in seconds) that a message can wait to be delivered before it expires. The default is 7200 seconds (two hours). The maximum value is only limited by the maximum value of an integer variable, which is around 2 billion. This setting is a desired property of the IoT Edge hub, which is stored in the module twin. You can configure it in the Azure portal or directly in the deployment manifest.

After an initial one-time sync with IoT Hub, IoT Edge devices can function indefinitely offline.
IoT Edge devices can function indefinitely offline after the initial, one-time sync. However, storage of messages depends on the time to live (TTL) setting and the available disk space for storing the messages.

Q: A developer for a company has discovered that they are losing data during periods when the local WiFi signal is lost and devices are offline. The developer investigates whether IoT Edge provides a solution for this problem. Which of the following answer choices accurately describes using Azure Blob Storage for IoT Edge devices?
* It uses a blob storage module to provide a block blob storage solution on your IoT Edge device.
* It uses the IoT Edge hub module to provide a block blob storage solution on your IoT Edge device.
* It uses IoT hub as the blob endpoint for any storage requests that are made by the IoT Edge device.

A: A blob storage module is used to provide a block blob storage solution for IoT Edge device and can be used for offline scenarios.

## Secure networks
By default, IoT Hub's hostnames map to a public endpoint with a publicly routable IP address over the internet. This allows different customers to share this IoT Hub public endpoint, and ensures that IoT devices connecting over wide-area networks and on-premises networks can access your hub. However, there are situations where you might want to restrict access to your Azure resources. Azure IoT solutions support both IP filtering and virtual networks to help secure access when needed.

For detailed information on securing network access, see the following resources:

* IoT Hub IP filtering: Use IP filters.
* Virtual network support: IoT Hub support for virtual networks with Private Link and Managed Identity
 
Q: When working to develop a secure IoT solution, which of the following tasks is typically assigned to the IoT developer role?
* Make hardware tamper proof.
* Protect against malicious activity.
* Follow secure software development methodology.

A: Among other things, the IoT developer is responsible for following a secure software development methodology.

Q: A company is implementing a threat modeling process to help improve the security of their IoT solution. Which of the following answer choices lists the three primary areas of consideration within the threat modeling process?
* Device security features, cloud security features, data security features.
* Security and privacy features, features whose failures are security relevant, features that touch a trust boundary.
* Hardware features, software features, cloud service features.

A: Security and privacy features, features whose failures are security relevant, features that touch a trust boundary. In addition to evaluating the solution as a whole, threat modeling for an IoT solution should focus on these three areas.
