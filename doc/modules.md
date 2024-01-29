# SOARCA Executer Module

SOARCA is extendable by modules. Modules allow for new steps in playbook and added capability. 

## Requirements
Modules should be build in GO or Python and contain the following components.

. CACAO template to allow their capability to extend coarse of actions playbooks.
. MQTT protocol implementation to communicate with the SOARCA executor.
. Module specifies which `variables` it exposes for return types. These `variables` should be defined when submitting a module. 


## Native modules in SOARCA
The following capability modules are defined in SOARCA:
 
- SSH
- HTTP-API
- OPEN-C2

All modules have an well known GUID for there target definition. SOARCA will also extent the `agent-target-type-ov` with the following vocab for `ssh`, `http-api` and `openc2` respectively.

- soarca-ssh--00010001-0001-0000-0000100010001
- soarca-http-api--00020001-0001-0000-0000100010001
- soarca-openc2--00030001-0001-0000-0000100010001


### SSH capability
Well know guid: `soarca-ssh--00010001-0001-0000-0000100010001`

This modules does not define variables as input. I will have the following output variables:

```json
{
    "__soarca_ssh_result__": {
        Type: "string",
        Name: "result",
        Value: "<output from command here>"
    }
}
```

If the connection to the target fail the structure will be set but be empty and an error will be returned. If no error occurred nil is returned.


### HTTP-API capability
Well know guid: `soarca-http-api--00020001-0001-0000-0000100010001`
```json
{
    "__soarca_http__result__": {
        Type: "string",
        Name: "result",
        Value: "<response from http-api here>"
    }
}
```

## OPEN-C2 capabilty
Well know guid: `soarca-openc2--00030001-0001-0000-0000100010001`

T.B.D.



## Protocol buffer interface (about to change | not implemented)

Protocol buffer 


```proto
syntax = "proto3";
package module;

message Command {
    repeated string command = 1;
    repeated string variable = 2;
    optional Result result = 3;
}

message Return {
    enum Result {
        OK = 0;
        RESPONSE = 1;
        ERROR = 2;
    }
    optional Result result = 1
    optional int code = 2;
    optional string message = 3;
}
```


## Variables
These `variables` are available within playbooks that use the module after it has outputted these variables. Varianames are __mod_$module_id$$_variable__ (example usage: virustotal returns a true/false for a malicious url and the affected ip. The variables would be __mod_virustotal_malicious__ and __mod_virustotal_ip__.)

## Project structure
soarca
module_meta.json
protocol.proto
main.py


## Loading your module

- Needs module ID, some string that uniquely identifies the module
- Needs return/ result typ definition
- Needs definition of exposed variables
- etc.
