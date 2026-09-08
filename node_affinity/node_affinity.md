This is like an upgrade for the NodeSelector
this is also done with labels and selectors

say for eample 
label -> disk =hdd
label -> disk =ssd

so affinity can be =ssd , !=ssd , in (ssd,hdd)

so if someone changes the label , once the pod is schedules in taint and toleration the pod would have been removed 

but here there are a few properties

requiredDuringSchedulingIgnoredDuringExecution => must satisy the condition
preferredDuringSchedulingIgnoredDuringExecution => it prefers if the condition can be matched

meaning if there is any change after the schedule it wont be removed and only be affecting the newly scheuled pods


| Operator       | Meaning                                             |
| -------------- | --------------------------------------------------- |
| `In`           | Label value **matches one of** the specified values |
| `NotIn`        | Label value **does not match** the specified values |
| `Exists`       | Label **exists**                                    |
| `DoesNotExist` | Label **does not exist**                            |
| `Gt`           | Label value is **greater than** the specified value |
| `Lt`           | Label value is **less than** the specified value    |

Operator types
