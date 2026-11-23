# Java CWE-209 Combined Test Suite

## Overview
This consolidated folder contains all 5 Java CWE-209 (Information Exposure Through an Error Message) test cases combined into a single project structure with one unified pom.xml file.

## Consolidation Details..

### Before
- `java-cwe209-01/` with pom.xml (Jakarta Servlet, SLF4J)
- `java-cwe209-02/` with pom.xml (Jakarta Servlet)
- `java-cwe209-03/` with pom.xml (Jakarta Servlet, Jackson, Spring)
- `java-cwe209-04/` with pom.xml (Javax Servlet)
- `java-cwe209-05/` with pom.xml (Javax Servlet)

Each had its own duplicate pom.xml file and used different servlet APIs.

### After
- `java-cwe209-combined/` - Single folder with one pom.xml file
  - `pom.xml` - Consolidated Maven configuration with all dependencies
  - `src/main/java/`
    - `StackTraceServlet_Test01.java` - Stack trace exposure via exception
    - `AuthServlet_Test02.java` - Security exception message exposure
    - `JsonServlet_Test03.java` - JSON parsing error message exposure
    - `StackTraceServlet_Test04.java` - Stack trace from division by zero
    - `SystemPropertyServlet_Test05.java` - Runtime exception message exposure

## Key Changes

✅ **Single pom.xml file** - Unified dependencies for all servlet APIs and frameworks
✅ **Unified folder structure** - All tests under one parent directory
✅ **Clear naming convention** - Each Java class named with `_Test0X` suffix
✅ **Multi-API support** - Includes both javax.servlet and jakarta.servlet

## File Structure

```
java-cwe209-combined/
├── pom.xml
├── README.md
└── src/main/java/
    ├── StackTraceServlet_Test01.java
    ├── AuthServlet_Test02.java
    ├── JsonServlet_Test03.java
    ├── StackTraceServlet_Test04.java
    └── SystemPropertyServlet_Test05.java
```

## Maven Configuration

The `pom.xml` includes unified dependencies for:
- **Servlet API (javax)** - For Tests 04, 05
- **Servlet API (jakarta)** - For Tests 01, 02, 03
- **SLF4J** - For logging support
- **Jackson** - For JSON parsing in Test 03
- **Spring Web** - For utility functions in Test 03

## Usage

To compile all test cases:
```bash
cd java-cwe209-combined
mvn clean compile
```

To package all test cases:
```bash
mvn clean package
```

## Test Case Descriptions

| Test | Class | Servlet API | Vulnerability Type | Exception Type |
|------|-------|-------------|--------------------|-----------------|
| 01 | StackTraceServlet_Test01 | Jakarta | Stack trace exposure | ArithmeticException |
| 02 | AuthServlet_Test02 | Jakarta | Security exception message | SecurityException |
| 03 | JsonServlet_Test03 | Jakarta | JSON parse error message | Exception (Jackson) |
| 04 | StackTraceServlet_Test04 | Javax | Stack trace exposure | ArithmeticException |
| 05 | SystemPropertyServlet_Test05 | Javax | Runtime error message | RuntimeException |

## CWE-209 Vulnerability Details

**CWE-209: Information Exposure Through an Error Message**

All 5 test cases demonstrate information exposure vulnerabilities where exception messages, stack traces, or sensitive error information are exposed to users/clients. This can reveal:
- Internal application structure
- Framework and library versions
- Database structure and details
- File paths and server configuration
- Other sensitive system information

### Common Attack Patterns

**Stack Trace Exposure:**
```java
catch (Exception e) {
    e.printStackTrace(resp.getWriter());  // CWE-209: Exposes full stack trace
}
```

**Error Message Exposure:**
```java
catch (Exception e) {
    resp.getWriter().println("Error: " + e.getMessage());  // CWE-209: May expose sensitive info
}
```

### Real-World Impact
- **Information Disclosure:** Attackers learn about technology stack
- **Security Through Obscurity Failure:** Hides implementation details
- **Compliance Issues:** May violate data protection regulations
- **Attack Surface Expansion:** Reveals potential attack vectors

## Migration Notes

- All test files maintain their original vulnerability patterns
- Class names have been updated to match filenames for Java compilation standards
- The consolidated pom.xml includes dependencies for both Servlet APIs (javax and jakarta)
- Different servlet APIs are used across tests to demonstrate multi-API error exposure

## Build Verification

✅ **All 5 Java files compile successfully**
✅ **JAR artifact generated**: `java-cwe209-combined-1.0-SNAPSHOT.jar`
✅ **All classes included in artifact**

## Scanning with Veracode

To scan this consolidated test suite with Veracode CLI:

```bash
# From the test_cases directory
./veracode_QA_patch package \
  --source java-cwe209-combined \
  --type directory
```

Expected Results:
- 5 HIGH severity CWE-209 issues detected
- Multiple issues per test class (exception handling points)
- Typically located in catch blocks where exceptions are output
