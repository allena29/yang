## YANG Models for Cisco IOS-XR 6.1.2

The YANG files in this directory detail the native YANG models and deviations supported by IOS-XR 6.1.2 releases. The schemas here may also be retrieved from devices running IOS-XR 6.0.1 by using the NETCONF "get-schema" RPC as detailed in Section 3.1 of RFC 6022.

### OpenConfig and IETF Model Inclusion

For ease of reference, the IOS-XR 6.1.2 models in this directory also include copies of the revisions of OpoenConfig and IETF model files used by IOS-XR 6.1.2. It should be noted that compilation of the OpenConfig models using the [check-models.sh](check-models.sh) script will result in errors being listed. Cisco has chosen not to modify these models to allow error-free compilation.


### Compliance With "pyang --lint"

The native YANG models are not fully compliant with all IETF guidelines as exemplified by running the pyang tool with the ```--lint``` flag. The errors and warnings exhibited by running pyang with the ```--lint``` flag are currently deemed to be non-critical as they do not impact the semantic of the models or prevent the models being used as part of toolchains. A script has been provided, "check-models.sh", that runs pyang with ```--lint``` validation enabled, but ignoring certain errors. This allows the developer to determine what issues may be present.


### Revision Statements

From IOS-XR 5.3.2 and onwards, the revision statements embedded in the YANG files **should** accurately reflect whether or not a new revision has been introduced. However, there are some bugs. These will be noted by running the ```check-models.sh``` script with the ```-b``` option.

### Models Not Published Due To Syntax Errors

The following models have been removed due to compilation errors:

* Cisco-IOS-XR-ncs1k-macsec-ea-oper.yang
* Cisco-IOS-XR-ncs1k-macsec-ea-oper-sub1.yang
* Cisco-IOS-XR-ip-bfd-oper-sub1-deviations.yang

However, these models are still available foir direct dowbload from platforms supporting these models or deviation, and so problems may be experienced with client software dependent on the correctness of models, such as ODL or NSO.


### Backwards Compatibility Issues

It should be noted that some of the modules released in IOX-XR 6.1.2 break the backwards compatibility guidelines defined in RFC 6020 when compared to the same modules released in IOS-XR 6.1.2. This is because the "native" YANG modules for IOS-XR are generated from internal schema files that are an integral part of the implementation, and, as such, these can change in ways that break backwards compatibility per RFC 6020 guidelines when new features are introduced or when bugs are fixed. Thus, while we rigorously review the changes that impact the external YANG schema, Cisco cannot guarantee full backwards compatibility of these modules across releases.

However, when new versions of the native models are released, the [```check-models.sh```](check-models.sh) script, in conjunction with pyang, can be used to determine what technically incompatible changes may have occurred. Please run ```check.sh``` from this directory with pyang 1.5 or greater on your path thus:

```
$ ./check-models.sh -b 601
```

The script will check basic compilation using pyang (some open modules will be reported missing unless you include them on your pyang module path) and then run backwards compatibility checks against the model in the ```../601``` directory. Directories other that 601 may be specified.

### Native Model Issue

A number of differences have crept in to a small number of YANG modules supported in IOS-XR 6.0.1. The differences, platforms impacted and where to find the alternate versions of models are detailed below. If you are working with an impacted platform, you should copy the YANG models from the relevant ```.incompatible``` subdirectory into your working directory and use that set of models instead.

If you retrieve models directly from a device, you will get the correct set of models.

Finally, it should be noted that there some Cisco YANG extensions that are referred to in modules. These may be safely ignored, but developers should note that when downloading schemas from devices some file sizes may differ from the files in the YangModels/yang git repository. Most of the common models were sources from the ncs5500


A single native model is impacted,  Cisco-IOS-XR-aaa-locald-cfg, and details may be found in [this directory](Cisco-IOS-XR-aaa-locald-cfg.incompatible).

A sample difference is shown below:

```
--- asr9k-px/Cisco-IOS-XR-aaa-locald-cfg.tree	Thu Sep 22 07:21:13 2016
+++ asr9k-x64/Cisco-IOS-XR-aaa-locald-cfg.tree	Thu Sep 22 07:23:57 2016
@@ -5,12 +5,13 @@
    +--rw default-taskgroup?   string
 augment /a1:aaa:
    +--rw usernames
-      +--rw username* [name]
+      +--rw username* [ordering-index name]
          +--rw usergroup-under-usernames
          |  +--rw usergroup-under-username* [name]
          |     +--rw name    xr:Cisco-ios-xr-string
          +--rw secret?                      xr:Md5-password
          +--rw password?                    xr:Proprietary-password
+         +--rw ordering-index               int32
          +--rw name                         string
 augment /a1:aaa:
    +--rw taskgroups
```

Platforms that have a username list indexed solely by name are:

* asr9k-px
* hfr
* xrvr

The version of the model for these platforms is stored in the directory [Cisco-IOS-XR-aaa-locald-cfg.incompatible](Cisco-IOS-XR-aaa-locald-cfg.incompatible)
