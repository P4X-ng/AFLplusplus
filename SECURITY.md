# Security Policy

## Supported Versions

AFL++ follows a rolling release model. We recommend always using the latest version from the stable branch.

| Branch | Supported          |
| ------ | ------------------ |
| stable | :white_check_mark: |
| dev    | :white_check_mark: |
| older  | :x:                |

## Security Context

AFL++ (American Fuzzy Lop plus plus) is a fuzzing tool designed to run locally in development and testing environments. It is not intended to be exposed to untrusted networks or to process untrusted inputs in production environments.

### Threat Model

AFL++ is designed with the following security assumptions:

1. **Local Execution**: AFL++ runs on a local machine or in a controlled testing environment
2. **Trusted Operator**: The user running AFL++ is trusted
3. **Testing Context**: AFL++ is used to test other software, not deployed as a service
4. **Input Trust**: Configuration files and command-line arguments come from trusted sources

### Out of Scope

The following are considered out of scope for security vulnerabilities:

- Issues that require the attacker to have local code execution
- Denial of service through resource exhaustion (AFL++ is designed to use system resources)
- Issues in test cases or example code (these may intentionally contain vulnerabilities)
- Security issues in the programs being fuzzed (that's what AFL++ is designed to find)

## Reporting a Vulnerability

If you discover a security vulnerability in AFL++, please report it responsibly:

### For Critical Issues

If you believe you have found a critical security vulnerability that could affect users:

1. **Do NOT** open a public GitHub issue
2. Email the maintainers directly (see CONTRIBUTING.md for contact information)
3. Include:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if available)

### For Non-Critical Issues

For lower-severity issues or questions about security:

1. Open a GitHub issue with the `security` label
2. Provide detailed information about the concern
3. We'll assess and respond within 7 days

## Security Best Practices for AFL++ Users

When using AFL++, follow these security best practices:

### 1. Isolation

- Run AFL++ in isolated environments (containers, VMs)
- Use separate machines for fuzzing untrusted targets
- Limit network access from fuzzing environments

### 2. Resource Limits

- Set appropriate CPU, memory, and disk limits
- Monitor system resources during fuzzing campaigns
- Use AFL_NO_AFFINITY if necessary to control CPU usage

### 3. Input Validation

- Validate command-line arguments and configuration files
- Be cautious with imported fuzzing corpora from untrusted sources
- Review crash inputs carefully before investigating

### 4. File System Safety

- Use dedicated output directories for fuzzing results
- Implement proper permissions on fuzzing directories
- Regularly clean up old fuzzing artifacts

### 5. Sanitizer Usage

- Use AddressSanitizer (ASAN) when fuzzing for memory safety issues
- Enable UndefinedBehaviorSanitizer (UBSAN) for additional checks
- Consider MemorySanitizer (MSAN) for uninitialized memory issues

## Known Security Considerations

### String Operations

AFL++ uses standard C string operations (`strcpy`, `strcat`, `sprintf`) in some locations. These are safe in the current implementation because:

1. Buffer sizes are carefully calculated
2. Input lengths are constrained by AFL++ itself
3. Many operations include post-operation length checks
4. AFL++ runs as a local tool with trusted inputs

### Memory Management

AFL++ uses custom memory allocators that track allocations and can detect leaks. The memory management is designed for the fuzzing use case and may not be suitable for all security-critical applications.

### Concurrency

AFL++ supports parallel fuzzing through multiple processes. File system race conditions are possible but are mitigated by:

- Using process IDs in file names
- Atomic file operations where possible
- Lock files for critical sections

## Security Updates

We take security seriously. When security issues are identified:

1. We assess the severity and impact
2. We develop and test a fix
3. We release the fix in the stable branch
4. We notify users through:
   - GitHub Security Advisories (for critical issues)
   - Release notes
   - Project documentation

## Attribution

We appreciate security researchers who responsibly disclose vulnerabilities. With your permission, we'll acknowledge your contribution in:

- The project's security advisory
- Release notes
- CHANGELOG.md

## Questions?

If you have questions about this security policy or AFL++ security in general:

1. Open a discussion on GitHub
2. Consult the documentation in `docs/`
3. Reach out to the maintainers

---

**Last Updated:** 2025-12-22  
**Version:** 1.0
