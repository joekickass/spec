# Workload

The `workload.yml` file allows a user to define functional/load/performance testing workloads to be executed for the different test strategies. The goal of this spec is to define a common set of workload properties that are relevant to all testing tools as well as an extension mechanism that allows individual tools to define tool specific settings.

Workload files are defined on the service level for each individual testing tool that integrates with Keptn, e.g: it could live in the myservice/jmeter or myservice/neoload folder. 

## Workload file explained

A Keptn Test Service, e.g: JMeter, Neoload ... should use the workload.yml specification to control the execution of a test, e.g: script, properties, ... but also test result reporting, e.g: evaluated timeframe, error reporting ...

Here is an overview of the core elements
**teststrategy**: defines for which test strategy this workload should be executed

**description**: just a logical name for the test

**script**: this section is very tool specific. It can just be a script name or a reference to test project, scenario ...

**properties**: this is a list of properties that should be passed to the executed test

**validations**: (optional) can be used to define when a test is sending a resul=pass vs result=fail, e.g: return fail if the overall failure rate of the test is >50%

**infrastructure**: (optional) some testing tools can provision infrastructure on demand. This is the place to define what test infrastructure is needed

**teststages** (optional) depending on the test you may have a warm-up, ramp-up, actual load test, cool-down phase, ... This allows a user to define what the actual load test phase is as this should be the timeframe used by the lighthouse service when retrieving the SLIs

## Example of a workload.yml file for JMeter

Here is an example of such a workload file defining JMeter specific values for individual test strategies.

```yaml
workloads:
  - teststrategy: performance
    description: PerformanceTest
    script: 
      - file: jmeter/load.jmx
    properties:
      - server_host: $SERVERURL
        server_port: $PORT
        protocol: $PROTOCOL
        vuser: 100
        loopcount: 10
        thinktime: 250
    infrastructure:
    validations:
      - acceptederrorrate: 1.0
    teststages:
      - ramp_up: 60s
        cool_down: 60s
  - teststrategy: performance_light
    description: PerformanceTest with 50 users
    script: 
      - file: jmeter/load.jmx
    properties:
      - server_host: $SERVERURL
        server_port: $PORT
        protocol: $PROTOCOL
        vuser: 50
        loopcount: 10
        thinktime: 250
        acceptederrorrate: 1.0
    teststages:
      - ramp_up: 60s
        cool_down: 60s        
```

## Example of a workload.yml file for Neotys

Here is the same example for Neotys.

```yaml
workloads:
  - teststrategy: performance
    description: BasicCheck
    script: 
      - repository: https://yourreposistory/project.git
        branch: master
        project:
        - path: /tests/neoload/load_template/load_template.nlp
        - path: /tests/neoload/catalogue_neoload.yaml
        scenario: BasicCheck
    properties:
      - server_host: $SERVERURL
        server_port: $PORT
        protocol: $PROTOCOL
    infrastructure:
      local_LG:
      - name: lg1
      populations:
      - name: BasicCheck
        lgs:
        - name: lg1
  - teststrategy: performance_light
    description: PerformanceLight
    script: 
      - repository: https://yourreposistory/project.git
        branch: master
        project:
        - path: /tests/neoload/load_template/load_template.nlp
        - path: /tests/neoload/catalogue_neoload.yaml
        scenario: PerformanceLight
    properties:
      - server_host: $SERVERURL
        server_port: $PORT
        protocol: $PROTOCOL
    infrastructure:
      local_LG:
      - name: lg1
      populations:
      - name: PerformanceLight
        lgs:
        - name: lg1            
```


