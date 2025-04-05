# Operations
Kubernetes follow this command syntax
```
kubectl [command] [type] [name] [flags]
```
where:
`command` - what you want it to do
`type` - where your want it act on
`name` - specific object you want to check
`flags` - optional variables
--
## Commands
1. `apply/create` - create resources
2. `run` - start a pod from an image
3. `explain` - documentation of resources
4. `delete` - delete resources
5. `get` - list resources
6. `describe` - detailed resource information. Useful for troubleshooting
7. `exec` - execute a command on an container
8. `logs` - view logs on a container

## Types
1. `nodes`
2. `pods`
3. `services` 

## Output
1. `wide` - output additional info
2. `yaml` - YAML formatted API object
3. `json` - JSON formatted API object
4. `dry-run` - print object without sending it to the API Server
