# Simple local log4j vulnerability scanner

(Written in Go because, you know, "write once, run anywhere.")

This is a simple tool that can be used to find vulnerable instances of
log4j 1.x and 2.x (CVE-2019-17571, CVE-2021-44228) in installations of
Java software such as web applications. JAR and WAR archives are
inspected and class files that are known to be vulnerable are flagged.
The scan happens recursively: WAR files containing WAR files
containing JAR files containing vulnerable class files ought to be
flagged properly.

This tool currently checks for known build artifacts that have been
obtained through Maven. From-source rebuilds as they are done for
Linux distributions may not be recognized.

Binaries for x86_64 Windoes, Linux, MacOSX for tagged releases are
provided via the
[Releases](https://github.com/hillu/local-log4j-vuln-scanner/releases)
page.

# Usage

``` console
$ ./log4j-vuln-scanner [--verbose] [--v1] [--exclude /path/to/exclude …] /path/to/app1 /path/to/app2 …
```

The `--verbose` flag will show every .jar and .war file checked, even if no problem is found.

The `--quiet` flag will supress output except for indicators of a known vulnerability.

The `--v1` flag will _exclude_ checks for older log4j 1.x vulnerabilities.


If class files indicating one of the vulnerabilities are found,
messages like the following are printed to standard output:
``` console
./local-log4j-vuln-scanner - a simple local log4j vulnerability scanner

indicator for vulnerable component found in /path/to/vuln/log4shell-vulnerable-app-0.0.1-SNAPSHOT.war::WEB-INF/lib/log4j-core-2.14.1.jar (org/apache/logging/log4j/core/net/JndiManager$JndiManagerFactory.class): log4j 2.14.0-2.14.1
indicator for vulnerable component found in /path/to/vuln/log4shell-vulnerable-app-0.0.1-SNAPSHOT.war::WEB-INF/lib/log4j-core-2.14.1.jar (org/apache/logging/log4j/core/net/JndiManager.class): log4j 2.14.0-2.14.1
Scan finished
```

# Building from source

Install a [Go compiler](https://golang.org/dl) and just run `go build`
in the checked out repository.

# License

GNU General Public License, version 3

# Author

Hilko Bengen <<bengen@hilluzination.de>>
