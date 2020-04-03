# Workload

The `workload.yml` file allows a user to define functional/load/performance testing workloads to be executed for the different test strategies. The goal of this spec is to define a common set of workload properties that are relevant to all testing tools as well as an extension mechanism that allows individual tools to define tool specific settings.

Workload files are defined on the service level for each individual testing tool that integrates with Keptn, e.g: it could live in the myservice/jmeter or myservice/neoload folder. 

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
    validations:
      - acceptederrorrate: 1.0
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
        vuser: 100
        loopcount: 10
        thinktime: 250
    infrastructure:
      local_LG:
      - name: lg1
      populations:
      - name: BasicCheck
        lgs:
        - name: lg1
  - teststrategy: performance_light
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
        vuser: 50
        loopcount: 10
        thinktime: 250
    infrastructure:
      local_LG:
      - name: lg1
      populations:
      - name: BasicCheck
        lgs:
        - name: lg1            
```


