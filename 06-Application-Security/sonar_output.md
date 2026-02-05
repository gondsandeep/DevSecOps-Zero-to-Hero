1️⃣ SonarQube real-time scan output (CLI example) 
-------------------------------------------------
================================================================================
 SAST ENTERPRISE SECURITY ANALYZER v10.3.2
================================================================================
 Project            : payment-service
 Application Tier   : Backend API
 Language Profile   : Python Secure Coding Ruleset
 Branch             : feature/security-hardening
 Commit SHA         : 9f2c1ab7d3
 Scan Trigger       : CI Pipeline (Pull Request)
 Scan Start Time    : 2026-02-06T11:58:42Z
================================================================================

[PHASE 1/6] Source Initialization
---------------------------------
INFO  Indexed 86 source files
INFO  Loaded 412 security rules
INFO  Enabled Compliance Packs: OWASP Top 10, CWE Top 25, PCI-DSS 4.0
INFO  Preparing taint-analysis engine
STATUS ✔ COMPLETED (2.1s)

[PHASE 2/6] Secrets & Configuration Scan
---------------------------------------
CRITICAL | Rule SAST-SEC-001 | CWE-798 | Hardcoded Secret
File: config/settings.py:12
Evidence: API_KEY = "sk_live_9x82KJ..."
Risk: Credential exposure → account takeover
Remediation: Move to environment variable or secrets manager
Compliance Impact: OWASP A2 / PCI-DSS 3.5

STATUS ✔ COMPLETED (1.3s)

[PHASE 3/6] Injection & Code Execution Analysis
----------------------------------------------

CRITICAL | Rule SAST-INJ-004 | CWE-89 | SQL Injection
File: db/user_repo.py:44
Flow: user_input → string concat → SQL execution
Code: "SELECT * FROM users WHERE id=" + user_id
Exploitability: HIGH
Fix: Parameterized query / ORM binding

CRITICAL | Rule SAST-EXEC-002 | CWE-94 | Arbitrary Code Execution
File: scripts/dynamic_runner.py:9
Code: eval(user_code)
Impact: Full remote code execution

MAJOR | Rule SAST-CMD-001 | CWE-78 | OS Command Injection
File: utils/backup.py:27
Code: os.system("tar -czf " + filename)
Fix: subprocess.run([...], shell=False)

STATUS ✔ COMPLETED (3.8s)

[PHASE 4/6] Deserialization & Parsing Risks
------------------------------------------

CRITICAL | Rule SAST-DESER-001 | CWE-502 | Insecure Deserialization
File: cache/session_loader.py:18
Code: pickle.loads(data)
Impact: Arbitrary object execution

CRITICAL | Rule SAST-YAML-001 | CWE-20 | Unsafe YAML Load
File: config/yaml_loader.py:15
Code: yaml.load(stream)
Fix: yaml.safe_load(stream)

CRITICAL | Rule SAST-PATH-003 | CWE-22 | Path Traversal in Archive Extraction
File: utils/archive.py:33
Code: tarfile.extractall(path)
Impact: Overwrite system files outside target dir

STATUS ✔ COMPLETED (2.9s)

[PHASE 5/6] Cryptography & Transport Security
--------------------------------------------

MAJOR | Rule SAST-TLS-002 | CWE-295 | SSL Verification Disabled
File: integrations/payment_client.py:52
Code: requests.get(url, verify=False)
Risk: MITM attack

MAJOR | Rule SAST-CRYPTO-001 | CWE-327 | Weak Hash Algorithm
File: auth/hash_utils.py:21
Code: hashlib.md5(password.encode())
Required: bcrypt / Argon2

STATUS ✔ COMPLETED (1.7s)

[PHASE 6/6] File Handling & Race Conditions
------------------------------------------

MAJOR | Rule SAST-TMP-001 | CWE-377 | Insecure Temporary File
File: utils/temp_manager.py:11
Code: tempfile.mktemp()
Risk: Race condition → file hijacking
Fix: tempfile.NamedTemporaryFile(delete=True)

STATUS ✔ COMPLETED (1.1s)

================================================================================
 ENTERPRISE SECURITY SUMMARY
================================================================================
 Critical Vulnerabilities : 6
 Major Vulnerabilities    : 4
 Medium / Minor           : 0
 Total Security Issues    : 10

 Exploitable Attack Paths : 4 confirmed
 Data Exposure Risk       : HIGH
 Remote Code Execution    : POSSIBLE
 Compliance Status        : ❌ NON-COMPLIANT (OWASP / PCI-DSS)

--------------------------------------------------------------------------------
 QUALITY GATE RESULT : ❌ FAILED
 Reason              : Presence of CRITICAL exploitable vulnerabilities
 Build Policy        : BLOCK DEPLOYMENT TO STAGING / PRODUCTION
--------------------------------------------------------------------------------

[REPORTING]
 SARIF Report  : artifacts/security/sast-report.sarif
 HTML Report   : artifacts/security/sast-report.html
 PDF Report    : artifacts/security/sast-report.pdf
 SonarQube URL : https://sonarqube.company.local/dashboard?id=payment-service

================================================================================
 CI PIPELINE STATUS: FAILED
 Security approval required before merge.
================================================================================
 Scan დასრულებული | Total Duration: 13.2s
================================================================================


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
