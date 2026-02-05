1️⃣ SonarQube real-time scan output (CLI example)
-------------------------------------------------
INFO: Scanner configuration file: /opt/sonar-scanner/conf/sonar-scanner.properties
INFO: Project root configuration file: /workspace/sonar-project.properties
INFO: SonarScanner 5.0.1
INFO: Java 17.0.8 Ubuntu (64-bit)
INFO: Linux 5.15.0
INFO: User cache: /home/user/.sonar/cache

INFO: Analyzing on SonarQube server 10.3.0
INFO: Default locale: "en_US", source code encoding: "UTF-8"
INFO: Load global settings
INFO: Load plugins index
INFO: Load project settings
INFO: ------------- Run sensors on module my-app
INFO: Load metrics repository
INFO: Sensor JavaSensor [java]
INFO: 120 source files to be analyzed
INFO: 120/120 source files have been analyzed

INFO: Sensor JaCoCo XML Report Importer [jacoco]
INFO: Importing coverage report /workspace/target/site/jacoco/jacoco.xml
INFO: Coverage report successfully imported

INFO: Analysis report generated in 2.5s, dir size=1.8 MB
INFO: Analysis report uploaded in 1.2s
INFO: ANALYSIS SUCCESSFUL, you can browse:
INFO: http://localhost:9000/dashboard?id=my-app
INFO: Note that you will be able to access the updated dashboard once the server has processed the submitted analysis report

SonarQube Issues Report (Sample)

Project: my-app
Scan Date: 06-Feb-2026
Quality Gate: ❌ Failed
Total Issues: 28

| Metric                 | Value |
| ---------------------- | ----- |
| Bugs                   | 3     |
| Vulnerabilities        | 1     |
| Code Smells            | 24    |
| Coverage               | 62%   |
| Duplications           | 5.2%  |
| Maintainability Rating | B     |
| Reliability Rating     | C     |
| Security Rating        | D     |


1️⃣ Vulnerabilities (Security)
🔴 SQL Injection Risk

Rule: java:S3649

Severity: CRITICAL

File: UserRepository.java

Line: 27

Issue: User input concatenated into SQL query.

Impact: Attackers can execute arbitrary SQL.

Fix: Use PreparedStatement or ORM parameter binding.

String query = "SELECT * FROM users WHERE id=" + userId; // unsafe

2️⃣ Bugs (Reliability)
🟠 Possible Null Pointer Dereference

Rule: java:S2259

Severity: MAJOR

File: UserService.java

Line: 42

Issue: user.getName() used without null check.

String name = user.getName();
return name.length(); // may throw NPE

🟠 Resource Leak – Stream Not Closed

Rule: java:S2095

Severity: MAJOR

File: FileProcessor.java

Line: 58

Fix: Use try-with-resources.

🟡 Incorrect String Comparison

Rule: java:S4973

Severity: MINOR

File: AuthController.java

Line: 33

Issue: == used instead of .equals().

3️⃣ Code Smells (Maintainability)
🟡 Method Too Long

Rule: java:S138

Severity: MINOR

File: OrderService.java

Line: 10

Issue: Method has 150+ lines.

Fix: Break into smaller methods.

🟡 Duplicate Code Block

Rule: java:S1192

Severity: MINOR

Files: EmailService.java, SmsService.java

Issue: Same validation logic repeated.

🟡 High Cognitive Complexity

Rule: java:S3776

Severity: MAJOR

File: PaymentProcessor.java

Issue: Complexity score 28 (limit: 15).

🟡 Missing Unit Tests on New Code

Metric: Coverage on new code 45% (<80%)

Impact: Quality Gate failure.

4️⃣ Summary by Severity
| Severity | Count |
| -------- | ----- |
| Critical | 1     |
| Major    | 5     |
| Minor    | 22    |
