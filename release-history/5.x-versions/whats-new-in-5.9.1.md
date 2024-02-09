# What's New in 5.9.1

This is a very small release with two changes.

* Update to Lucee 5.4.3.2
* Update bundled JRE to 11.0.20+8

Note Lucee 5.4.3.2 contains critical security patches which are outlined here:

[https://dev.lucee.org/t/lucee-critical-security-alert-august-15th-2023-cve-2023-38693/12893](https://dev.lucee.org/t/lucee-critical-security-alert-august-15th-2023-cve-2023-38693/12893)

The new Lucee version affects the core CLI runtime as well as the default server you get when running "server start" with no cfengine specified.  Possible compatibility issues related to the major bump in Lucee version:

* This Lucee version does not include Hibernate, so the Ortus Hibernate extension is installed.  We will stop doing this in 6.0
* This Lucee version has strict XML parsing settings on by default which may affect any servers you start which parse XML containing DTDs.

If you do run into XML errors, this code may help you in your Application.cfc, which allows DTDs, but still disallows XML external entities (XEE).

```
this.xmlFeatures={
	externalGeneralEntities: false,
	disallowDoctypeDecl: false
};
```

### Release notes

#### Task

* [COMMANDBOX-1609](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1609) Update bundled JRE to 11.0.20+8
* [COMMANDBOX-1610](https://ortussolutions.atlassian.net/browse/COMMANDBOX-1610) Update to Lucee 5.4.3.2
