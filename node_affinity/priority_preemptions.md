What is priority?
This decides which pod needs to be there before anyone else , and essentially giving an importance to those that need to be scheduled and the one for the eviction

Priority class gives a priority name and a value

there will be a default of the lower priority class

There is s preemption policy: never have the ability to evict pods 

And this can also be used as a priority preemption where the high priority can come in and kick out the less priority class ones 


kubectl get priorityclass
to check the priority class


| `preemptionPolicy`     | Can evict lower-priority running Pods? | Priority affects pending order? |
| ---------------------- | -------------------------------------: | ------------------------------: |
| `PreemptLowerPriority` |                                  ✅ Yes |                           ✅ Yes |
| `Never`                |                                   ❌ No |                           ✅ Yes |


we can see the preemption in the kubectl get events to see the events 

or kubectl logs -n kube-system kube-scheduler-<control-plane-node>


Pod scheduling and preemption are handled by the kube-scheduler, not the controller manager.

kubectl get events --sort-by=.lastTimestamp #json sorting and mapping 