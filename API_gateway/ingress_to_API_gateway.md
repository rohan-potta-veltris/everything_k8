How to migrate from ingress to api gateway?

There is a tool called ingress2gateway , install and then it will translate to everything 

Manual Mapping:
so if initially they go DNS -> ingress-> mapping -> pods 

it should be DNS -> API Gateway -> mapping -> pods 
so we first make the api gateway and re-route the dns to this and then delete the ingress , so that the users have the least amount of downtime

