What is CRD , this is a custom resource defination , this is used to make our own objects , this is like how the api-gateway was implemented

This is done to fill the blanks managed by k8 

This will be used if we need to make any custom defination like tracking and exportign any sort of metrics.

CRD is like a template and then we can call this with the help of a manifest of the CR and then we get our resource

Like dpeloyment is a CRD made by k8 and the manifest to call that deployment is a CR

The componenets:
CRD
CD 
Custom Controller => manages the lifecycle of the custom resource 

