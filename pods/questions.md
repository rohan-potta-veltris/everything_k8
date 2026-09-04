Q) Our pod has the error ImagePullBackOff error what does this mean?
Ans) This means that the error has to do from the pod while pulling the image, we can identify the error by saying kubectl describe pod pod_name this will show what the error is , whether it is due to lack of permission to get the image , or the image couldn't be resolved and the name was incorrect

Q) what is apiVersion , kind, metadata and spec in a pod and why are they used?


