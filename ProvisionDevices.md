

The Azure IOT Hub DPS platform provides secure, automated IOT device provisioning at scale. During the manufacturing process, each device is programmed with information required by DPS, including methods for securely identifying each device's identity. This information is then used to configure DPS for automatic provisioning. Once powered on, the device can locate DPS and then be automatically provisioned, including receiving its initial configuration. DPS acts as a broker between a new, unconfigured device and Azure IOT Hub. This eliminates the need to manually create devices in the Hub. 


The az iot dps link-hub update command is used to update an IOT hub that is linked to Azure IOT Hub DPS. This command is typically used to modify an allocation policy. DRS uses allocation policies to determine how IOT devices should be assigned to IOT hubs. 

DPS supports three allocation policies, which you can select when creating enrollments: lowest latency, evenly weighted distribution, and static configuration.

You can link an existing Azure IOT Hub DPS instance to an existing Azure IOT Hub from DPS. One of the benefits Of DRS is that it supports linking to multiple IOT hubs across multiple Azure subscriptions. In an environment where you have millions of IOT devices, you may want to allocate those devices evenly across multiple hubs to reduce contention and distribute management responsibilities. For example, you may want newly connected devices to be provisioned by the IOT hub with the lowest network latency, and presumably the best connectivity. You can use DPS allocation policies to do it automatically. The iothubowner access policy provides the permissions that DPS requires to interact with Azure IOT Hub. 

Azure IOT Hub DPS requires the registry read, registry write, and service connect permissions.

Azure IOT solution accelerators are complete, ready-to-deploy IOT solutions that implement common IOT scenarios, such as predictive maintenance or remote monitoring. You can use Azure IOT solution accelerators as a starting point to build your own IOT solutions, but they cannot authenticate 10T devices and subsequently register them in your Azure IOT Hub for a zero-touch auto-provisioning. 

DPS is a helper service for IOT Hub that enables zero-touch, just-in-time provisioning of millions of devices in a secure and scalable manner to the right Azure IOT hub without requiring human intervention.

Azure IOT Edge enables IOT Edge runtime on your IOT devices to run containerized Azure, third-party or your own custom applications locally, as well as to monitor and manage your IOT devices remotely. However, you cannot use Azure IOT Edge for a zero-touch auto-provisioning Of the factory IOT devices in your designated Azure IOT Hub. 

You should use Azure IOT Hub Device Provisioning Service (DRS) instead. DPS is a helper service for IOT Hub that enables zero-touch, just-in-time provisioning Of millions Of devices in a secure and scalable manner to the right Azure 10T hub without requiring human intervention. 
