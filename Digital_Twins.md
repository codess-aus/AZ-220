"Azure Digital Twins" refers to the Azure Digital Twins service as a whole. 
"Digital twin(s)" or just "twin(s)" refers to the digital entities represented within your solution.

Azure Digital Twins can be used to build customized, connected solutions that:

* Model any environment, and bring digital twins to life in a scalable and secure manner.
* Connect assets such as IoT devices and existing business systems.
* Use a robust event system to build dynamic business logic and data processing.
* Integrate with Azure data, analytics, and AI services to help you track the past and then predict the future.

Digital twins (in Azure Digital Twins) and device twins (in IoT Hub) may sound similar, but they are two different things. 
IoT Hub device twins often focus on describing the aspects and capabilities of a device itself, while digital twins are more conceptual representations that can store user-defined insights about a digital entity or many related entities (entities that could be representing a device or many related devices). 
It is also worth noting that IoT Hub device twins can be connected to Azure Digital Twins (the service) as part of an end-to-end solution that represents devices across services.

What is the relationship between an Azure Digital Twins model and a digital twin?
A digital twin is an instance of an ADT model.

What is the name of the coding format used to define Azure Digital Twins models?
Digital Twins Definition Language (DTDL) is the name of the coding format used to define Azure Digital Twins models.

Once a model is uploaded to your Azure Digital Twins instance, the entire model interface is immutable, which means there is no traditional "editing" of models. Azure Digital Twins also does not allow reupload of the same model.

Instead, if you want to make changes to a model - such as updating displayName or description - the way to do this is to upload a newer version of the model.

o create a new version of an existing model, start with the DTDL of the original model. Update, add, or remove the fields you would like to change.

Next, mark the file as a newer version of the model by updating the ID field of the model. The last section of the model ID, after the ;, represents the model number. To indicate that this is now a more-updated version of this model, increment the number at the end of the ID value to any number greater than the current version number.

Then, upload the new version of the model to your instance.

This version of the model will then be available in your instance to use for digital twins. It does not overwrite earlier versions of the model, so multiple versions of the model will coexist in your instance until you remove them.

Impact on twins:
When you create a new twin, since the new model version and the old model version coexist, the new twin can use either the new version of the model or the older version.

This also means that uploading a new version of a model does not automatically affect existing twins. The existing twins will remain instances of the old model version.

You can update these existing twins to the new model version by patching them.

Models can also be removed from the service in one of two ways:

Decommissioning: Once a model is decommissioned, you can no longer use it to create new digital twins. Existing digital twins that already use this model aren't affected, so you can still update them with things like property changes and adding or deleting relationships.
Deletion: This will completely remove the model from the solution. Any twins that were using this model are no longer associated with any valid model, so they're treated as though they don't have a model at all. You can still read these twins, but won't be able to make any updates on them until they're reassigned to a different model.
These are separate features and they do not impact each other, although they may be used together to remove a model gradually.

