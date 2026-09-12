What are the restart policies for the CronJob
valid values: "OnFailure", "Never"
why do they exist and when are they needed?


CronJobs will always make a new pod and won't reuse the created pod