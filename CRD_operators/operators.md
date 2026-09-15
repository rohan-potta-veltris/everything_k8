In Kubernetes, an Operator is a piece of software that automates the management of an application by using Kubernetes APIs.

we deploy the operator and tell what the desired state will be , this operator is used for configs deployments and scaling and updates on the app.
This is the automation for the k8 as well

You don't need an Operator just to manage a Deployment — Kubernetes already has a Deployment controller. You build an Operator when you want to add application-specific automation around Deployments or other resources.

This usually has the application level logic and things 

We can install this via kubectl , helm or OLM (Operator lifecycle manager)

To write the custom operator is 
helm , ansible , go is prefeered and most common
