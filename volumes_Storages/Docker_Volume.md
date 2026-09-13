docker layers are immutable meaning they are a read only layers
and then when we go into the contianer and make changes these are write changes but they are not persistent because they live within the pod , and this is possible because they are a copy of the read only layers

To make the data persistent we use volumes , these work with storage drivers of overlay2


If we want to handle the volume seprate and independent of the container we use
docker create volume volume_name , and this will be in the path /var/lib/docker/volumes
or we can mention where we want it 

now if we use 
docker run -v volume_name:path_on_contianer

but docker run -v path_on_host:path_on_container this will be created if it doenst exist already

Mount bind is when we take the exisitng foler from host to the contianer usign the above command    
docker run -v C:\myapp:/app nginx