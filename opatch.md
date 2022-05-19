# OPatch Install & Apply Patches

How to apply patches by OPath

### OPatch Install

* Download OPatch (find OPatch link from Patch README)
* Extract zip file
* Install OPatch to Oracle_Home folder
* Add OPatch path to System path

```shell
<path to java>\java.exe  -jar opatch_generic.jar -silent oracle_home=C:\Oracle\JDEV12214\Oracle_Home
```

### Apply Patches

* Open command prompt with Administrator rights
* Go to extracted patch folder
* Run OPatch
```shell
opatch apply
```
