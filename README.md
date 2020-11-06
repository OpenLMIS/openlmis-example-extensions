# OpenLMIS Example Extensions
This example is a Docker image containing extensions of OpenLMIS services. It is meant to demonstrate how extensions are added to openlmis-ref-distro.


## Quick start
1. Fork/clone this repository from GitHub.
 ```shell
 git clone https://github.com/OpenLMIS/openlmis-example-stockmanagement-validator-extension.git
 ```
2. Add an environment file called `.env` to the root folder of the project, with the required 
project settings and credentials. For a starter environment file, you can use [this 
one](https://raw.githubusercontent.com/OpenLMIS/openlmis-ref-distro/master/settings-sample.env). eg:
 ```shell
 curl -o .env -L https://raw.githubusercontent.com/OpenLMIS/openlmis-ref-distro/master/settings-sample.env
 ```

3. Start up the application.
 ```shell
 docker-compose -f ref-distro-example-docker-compose.yml up
 ```
4. Check if the application behavior has changed according to the implemented extension point. 

5. Bean of implemented extension point should be also visible in docker-compose logs.
   To see extended logs add this loggers to the env file of ref-distro.
```
 logging.level.org.springframework.beans.factory=DEBUG
 logging.level.org.springframework.core.io.support=DEBUG
 logging.level.org.springframework.context.annotation=DEBU
```


## Integration with openlmis-ref-distro
1. Fork/clone `openlmis-ref-distro` repository from GitHub.
 ```shell
 git clone https://github.com/OpenLMIS/openlmis-ref-distro.git
 ```
2. Start up openlmis-ref-distro.
 ```shell
    docker-compose -f docker-compose.openlmis-stockmanagement-validator-extension.yml up
 ```
 
## <a name="extensionpoints">Adding extension points</a>
1. Add extension to the "dependencies" configuration in build.gradle:
```
    extension "org.openlmis:openlmis-stockmanagement-validator-extension:0.0.1-SNAPSHOT"
```
2. Modify extensions.properties with name of the extended component.
```
    AdjustmentReasonValidator=ExtensionAdjustmentReasonValidator
    FreeTextValidator=ExtensionFreeTextValidator
    UnpackKitValidator=ExtensionUnpackKitValidator
```


## <a name="configuringrefdistro">Configuring openlmis-ref-distro</a>
The Reference Distribution is configured to use extension modules by defining a named volume that is common to the service and partner image. 
```
volumes:
  example-extensions:
    external: false
```
The shared volume contains extension jars and extension point configuration. The role of this image is to copy them at start-up, so they may be read by the service.

An example configuration can be found in openlmis-ref-distro as `docker-compose.openlmis-stockmanagement-validator-extension.yml`.
