You build a new Azure IOT Edge module that should run continuously on your company IOT Edge devices. You need to choose the right restart policy module for the IOT Edge agent to restart your module if it shuts 
down for any reason. 


You should use the always rnodule. This restart policy instructs the Azure IOT Edge agent to automatically restart the module to support its continuous run on the edge device if it shuts down for any reason, even 
cleanly. 

You should not use the on-failure module. This restart policy instructs the Azure IOT Edge agent to restart the module only if it crashes. However, the module will not be restarted if it shuts down cleanly without 
errors. 

You should not use the on-unhealthy module. This restart policy instructs the agent to restart the module only if it crashes or returns an unhealthy status. Implementation of the health status is specific to each 
module, and you can define it as a part of your custom module code. 

You should not use the never module. Azure IOT Edge agent never restarts the module no matter the reason Of its shutdown. 
