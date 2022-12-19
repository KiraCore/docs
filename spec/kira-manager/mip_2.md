# [⏎](README.md#Roadmap) MIP_2
> Dependencies Installation Manager

`Dependencies Manager` is a task that should be launched during each start of the KM service. Its objective is to verify versions of all the the software installed on the instance, compare it against predefined/expected versions and finally upgrade or install if needed.

Some of the essential dependencies should include:
* Golang
* firewalld (firewall-cmd)
* Docker
* KIRA Manager (self)

It's essential that KM and each individual dependency setup routine has a dedicated logs (eg. dumped to the `/kira/logs` dir) and that each dependency has its own log file allowing for future debugging. Note that all log files should always be trimmed to the predefined maximum size to prevent uncontrollable disk space consumption (eg. something goes wrong and process continuously restarts while appending data to log files).

Dependencies Installation Manager might operate similarly to an NPM package manager, where easily accessible json can be part of its repository, eg.:

```
{
  "dependencies": [
    "golang": {
      "version": "^1.1.1",
      "install": "<path>",
      "upgrade": "<path>"
    },
    "docker": { ... }, ... ] }
```
The `version` should be tested against current version of the software (or lack of the software) and appropriate `install` or `upgrade` script should be launched if needed.

If the software version does not match the expected version the dependencies manager should gracefully stop all KM related services (eg. docker) and ensure the correct dependencies are safely setup (that includes circular dependencies used to run KM itself such as Golang). To prevent issues during circular dependency installation a dedicated systemd service can be created to prevent issues where instance suddenly restarts, is shut down corrupting the process or requires a reboot (eg. firewall)

