# CS305-Artemis-Financial

## Client and requirements

The client was Artemis Financial, a financial services company that wanted to modernize its operations. It runs custom software and wanted the latest, most secure security. My job was to refactor the starter Spring Boot project to add secure data verification and a secure connection for the web application. I generated an RSA 2048-bit keystore and a self-signed certificate with keytool, turned on HTTPS on port 8443, and added a /hash route that returns a SHA-256 checksum. I then tested the application and scanned its dependencies.

## What I did well, and why secure coding matters

I picked algorithms that hold up today. I used SHA-256 instead of MD5 or SHA-1, since real collisions have been found for both, and I used a 2048-bit RSA key because 1024-bit keys are no longer considered safe. I also looked past the code I wrote. The dependency scan showed that the starter project's 2020 libraries carried 295 known vulnerabilities across 16 dependencies, and I wrote that up with a fix plan instead of ignoring it.

Secure coding matters because the cost of a breach goes well beyond the code. A financial company that gets hit could lose hundreds of thousands of dollars, especially if most of its money is stored electronically. It would also put personal data at risk for clients and employees alike. The biggest long-term loss is trust, because people stop banking with a company that cannot protect them. Security protects the company's money, its people, and its reputation.

## Challenging and helpful parts

The hardest part was working with outdated tools. The dependency-check plugin in the starter project (version 5.3.0) tried to pull vulnerability data from feeds that no longer exist, so it failed with a 403 error. I updated it to version 12.1.0. The next run failed with a 401 error because one analyzer, the Sonatype OSS Index, now requires an account. I turned that analyzer off, and the main scan against the National Vulnerability Database ran fine. It was frustrating, but it taught me that security tools need maintenance too, and that newer versions of them often expect credentials or keys.

## Layers of security

I added three layers. The self-signed RSA certificate lets the server prove its identity and sets up the encrypted session. HTTPS with TLS encrypts traffic between the browser and the server, so someone who intercepts it cannot read or quietly change it. The SHA-256 checksum lets a user confirm that data did not change, though it does not prove who sent it. In production, I would add a keyed hash (HMAC) or a digital signature for that, and I would use a certificate signed by a trusted authority, so browsers stop showing the "Not secure" warning.

In the future, I would use OWASP Dependency-Check in the build pipeline to find vulnerable libraries, and I would review each finding before fixing or suppressing it, since some are false positives. I would also use manual code review and the vulnerability assessment process flow diagram to cover areas like input validation, APIs, error handling, and encapsulation.

## Making sure the code is functional and secure

I did three kinds of checks. I ran the application and confirmed it started cleanly on port 8443. I confirmed that the checksum shown in the browser matched what an independent SHA-256 tool produced for the same string. I also reviewed the code by hand for syntax, logic, and security problems. After refactoring, I checked for new vulnerabilities by rerunning the dependency scan. I added no new dependencies, and the new class only uses the Java standard library and Spring web annotations that were already in the project, so I introduced no new vulnerable libraries. The manual review also found two things to fix in a real deployment: the keystore password is stored in plain text, and the error handler should log failures on the server.

## Resources, tools, and practices for future work

I used Eclipse, Maven, keytool, Spring Boot, and OWASP Dependency-Check. The practices I will reuse are keeping secrets like passwords out of the code, validating all user input, returning generic error messages while logging details on the server, upgrading dependencies on a schedule, and testing with more than one method. I also learned to read a failed build error carefully before changing anything, since it usually says exactly what is wrong.

## What I could show employers

I could show this report as an example of a full security workflow: choosing the right cryptography, setting up HTTPS, running a dependency scan, and writing up the results with a clear fix plan. It also shows I can explain technical choices in plain language and be honest about what I did not fix, like the Spring Boot upgrade, and why.
