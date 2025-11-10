You can assign labels to nodes (older term)/agents (newer term).

You can assign same label to different nodes.
	Jenkins use one of the matching nodes based on availability and load.

You can run different stages on different nodes.
	Also you can use "agent none" at top of the pipeline to each stage to pick its own agent.
	BUT in freestyle job all build steps run on same agent.
	You can only do this in Declarative Pipeline.

Number of executors: number of threads to run jobs in parallel

At the beginning of setup a node, jenkins push the remoting.jar and it will run agent.
remoting.jar does all the processes.

The **"Jenkins master" is the main computer where Jenkins itself is installed and managing everything.** It schedules builds, provide web ui, manages jenkins jobs, monitor agents, running builds if no slaves are used (this is bad practice.)
It can also run jobs, but for scalability and best practices, jobs should ideally be delegated to agents.

