## Viewing Security Vulnerabilities

The `image vuln` command can be used to return a list of vulnerabilities found in the container image.

`anchore-cli image vuln INPUT_IMAGE VULN_TYPE`

The `INPUT_IMAGE` can be specified in one of the following formats:

- Image Digest
- Image ID
- registry/repo:tag

The `VULN_TYPE` currently supports:

- os: Operating System Package CVEs
- non-os: NPM, GEM, Java Archive (jar, war, ear) and Python PIP CVEs.
- all: Combination report containing both 'os' and 'non-os' vulnerability records. 

The system has been designed to incorporate 3rd party feeds for other vulnerabilites.

### Examples

To generate a report of OS package (RPM/DEB/APK) vulnerabilities found in the image including CVE identifier, Vulnerable Package, Severity Level, Vulnerability details and version of fixed package (if available).

`anchore-cli image vuln debian:latest os`

Currently the following the anchore-engine draws vulnerability data specifically matched to the following OS distros:

- Alpine
- CentOS
- Debian
- Oracle Linux
- Red Hat Enterprise Linux
- Ubuntu
- Amazon Linux 2

To generate a report of language package (NPM/GEM/Java/Python) vulnerabilities, the system draws vulnerability data from the NVD data feed, and vulnerability reports can be viewed using the 'non-os' vulnerability type:

`anchore-cli image vuln node:latest non-os`

**Note:** to enable 'non-os' scanning, the engine must be configured to sync the 'nvd' data feed - see [Initial Configuration]() and [Feeds]() for details.

To generate a list of all vulnerabilities that can be found, regardless of whether they are against an OS or non-OS package type, the 'all' vulnerability type can be used:

`anchore-cli image vuln node:latest all`

Finally, for any of the above queries, these commands (and other anchore-cli commands) can be passed the `--json` flag to output the data in JSON format:

`anchore-cli --json image vuln node:latest all`

### Next Steps

- [Evaluate the image]() against policies you create.
- Subscribe to receive [notifications]() when the image is updated, when the policy status changes or when new vulnerabilities are detected.





