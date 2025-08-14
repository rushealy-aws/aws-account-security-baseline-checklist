# AWS Account Security Baseline Checklist

A comprehensive checklist based on AWS prescriptive guidance to establish a secure foundation for your AWS account. This checklist follows the AWS Startup Security Baseline (AWS SSB) and AWS security best practices.

## Overview

This checklist is designed to help you implement foundational security controls that create a minimum security baseline for your AWS account. The controls are organized into categories focusing on identity and access management, logging and monitoring, data protection, and incident response. This list is follows the [AWS Startup Security Baseline for Securing Your AWS Account](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-startup-security-baseline/controls-acct.html), which is part of the [AWS Prescriptive Guidance](https://aws.amazon.com/prescriptive-guidance/) framework.

---

## 1. Root Account Security

### 1.1 Root User Protection
- [ ] **Enable MFA on root account**
  - Use a hardware MFA device (recommended) or virtual MFA device
  - Store backup codes in a secure location
  - Test MFA functionality after setup

- [ ] **Remove or secure root access keys**
  - Delete root access keys if they exist
  - Never share root access keys

- [ ] **Set strong root password**
  - Use a complex, unique password (minimum 14 characters)
  - Store password securely in a password manager
  - Do not reuse passwords from other accounts

- [ ] **Limit root account usage**
  - Use root account only for tasks that require root privileges
  - Document when and why root account is used
  - Create IAM users for day-to-day operations

### 1.2 Account Contact Information
- [ ] **Update account contact information**
  - Verify primary email address is monitored
  - Update billing contact information
  - Set up alternate contacts for security notifications
  - Configure security contact information

---

## 2. Identity and Access Management (IAM)

### 2.1 User Management
- [ ] **Implement federated access for human users**
  - Set up AWS IAM Identity Center (recommended)
  - Configure external identity provider integration
  - Use temporary credentials instead of long-term access keys
  - Implement single sign-on (SSO) where possible

- [ ] **Create individual IAM users when federation is not possible**
  - Create unique IAM user for each person
  - Avoid sharing IAM user credentials
  - Use descriptive usernames (e.g., firstname.lastname)

- [ ] **Enforce strong password policy**
  - Minimum password length: 14 characters
  - Require uppercase, lowercase, numbers, and symbols
  - Enable password expiration (90 days recommended)
  - Prevent password reuse (last 24 passwords)
  - Require password reset on first login

### 2.2 Multi-Factor Authentication (MFA)
- [ ] **Enable MFA for all IAM users**
  - Require MFA for console access
  - Use hardware tokens for privileged users
  - Configure virtual MFA for standard users
  - Document MFA device recovery procedures

- [ ] **Enforce MFA through policies**
  - Create IAM policies that deny access without MFA
  - Apply MFA requirements to all privileged operations
  - Test MFA enforcement policies

### 2.3 Access Keys and Credentials
- [ ] **Minimize use of long-term credentials**
  - Use IAM roles for applications and services
  - Implement temporary credentials where possible
  - Document legitimate use cases for access keys

- [ ] **Manage access keys securely**
  - Rotate access keys regularly (every 90 days)
  - Remove unused or inactive access keys
  - Monitor access key usage with credential reports
  - Never embed access keys in code or version control

### 2.4 Least Privilege Access
- [ ] **Implement principle of least privilege**
  - Grant minimum permissions necessary for job function
  - Use AWS managed policies as starting point
  - Create custom policies for specific use cases
  - Regularly review and audit permissions

- [ ] **Use IAM groups for permission management**
  - Create groups based on job functions
  - Assign users to appropriate groups
  - Manage permissions at group level
  - Avoid attaching policies directly to users

- [ ] **Implement IAM roles for cross-account access**
  - Use roles instead of sharing credentials
  - Configure trust relationships carefully
  - Implement external ID for third-party access
  - Monitor role usage and assumptions

### 2.5 Permission Boundaries and Guardrails
- [ ] **Implement permissions boundaries**
  - Set maximum permissions for IAM entities
  - Use boundaries to delegate permission management
  - Prevent privilege escalation

- [ ] **Use IAM Access Analyzer**
  - Enable IAM Access Analyzer in all regions
  - Review findings for unintended access
  - Generate least-privilege policies from access patterns
  - Monitor external access to resources

---

## 3. Logging and Monitoring

### 3.1 AWS CloudTrail
- [ ] **Enable CloudTrail in all regions**
  - Create organization-wide trail (if using AWS Organizations)
  - Enable multi-region trail for comprehensive coverage
  - Log management events in all regions
  - Configure data events for sensitive resources

- [ ] **Secure CloudTrail logs**
  - Store logs in dedicated S3 bucket
  - Enable S3 bucket versioning
  - Configure S3 bucket encryption (SSE-S3 or SSE-KMS)
  - Restrict access to CloudTrail logs

- [ ] **Enable log file integrity validation**
  - Enable log file validation for tamper detection
  - Monitor digest files for integrity verification
  - Set up alerts for validation failures

- [ ] **Configure CloudTrail log retention**
  - Set appropriate log retention period (minimum 1 year)
  - Configure S3 lifecycle policies
  - Consider archiving to Glacier for long-term retention

### 3.2 Amazon CloudWatch
- [ ] **Set up CloudWatch monitoring**
  - Send CloudTrail logs to CloudWatch Logs
  - Create custom metrics for security events
  - Set up dashboards for security monitoring

- [ ] **Configure security alerts**
  - Root account usage alerts
  - Failed console login attempts
  - Unauthorized API calls
  - Changes to security groups and NACLs
  - IAM policy changes
  - MFA device changes

### 3.3 AWS Config
- [ ] **Enable AWS Config**
  - Enable Config in all regions
  - Configure delivery channel to S3 bucket
  - Set up configuration history retention

- [ ] **Implement Config rules**
  - Root access key check
  - MFA enabled for root account
  - CloudTrail enabled
  - S3 bucket public access prohibited
  - Security group SSH/RDP restrictions

### 3.4 Additional Security Services
- [ ] **Enable Amazon GuardDuty**
  - Enable in all regions
  - Configure findings notifications
  - Review and respond to findings regularly

- [ ] **Set up AWS Security Hub**
  - Enable Security Hub in all regions
  - Enable security standards (AWS Foundational, CIS, PCI DSS)
  - Configure findings aggregation
  - Set up automated remediation where appropriate

---

## 4. Network Security

### 4.1 VPC Security
- [ ] **Configure VPC security groups**
  - Follow principle of least privilege
  - Avoid using 0.0.0.0/0 for inbound rules
  - Use specific ports and protocols
  - Regularly audit security group rules

- [ ] **Implement Network ACLs**
  - Configure subnet-level access controls
  - Use NACLs as additional security layer
  - Document NACL rules and purposes

- [ ] **Enable VPC Flow Logs**
  - Enable flow logs for VPCs, subnets, and ENIs
  - Store logs in CloudWatch Logs or S3
  - Monitor for suspicious network activity

### 4.2 Public Access Controls
- [ ] **Restrict public access**
  - Avoid public S3 buckets unless necessary
  - Use CloudFront for public content delivery
  - Implement proper access controls for public resources

---

## 5. Data Protection

### 5.1 Encryption
- [ ] **Enable encryption at rest**
  - Use AWS KMS for key management
  - Enable S3 bucket encryption by default
  - Encrypt EBS volumes
  - Enable RDS encryption

- [ ] **Enable encryption in transit**
  - Use HTTPS/TLS for all communications
  - Configure SSL/TLS certificates properly
  - Disable insecure protocols (SSLv3, TLS 1.0/1.1)

### 5.2 Key Management
- [ ] **Implement AWS KMS best practices**
  - Use customer-managed keys for sensitive data
  - Implement key rotation policies
  - Control key usage with IAM policies
  - Monitor key usage with CloudTrail

### 5.3 Backup and Recovery
- [ ] **Implement backup strategies**
  - Enable automated backups for databases
  - Use AWS Backup for centralized backup management
  - Test backup restoration procedures
  - Store backups in separate regions

---

## 6. Compliance and Governance

### 6.1 AWS Organizations (if applicable)
- [ ] **Set up AWS Organizations**
  - Create organizational units (OUs) by function
  - Implement service control policies (SCPs)
  - Enable consolidated billing
  - Set up cross-account roles

### 6.2 Tagging Strategy
- [ ] **Implement consistent tagging**
  - Define tagging standards
  - Tag resources for cost allocation
  - Use tags for security and compliance
  - Automate tagging where possible

### 6.3 Cost Management
- [ ] **Set up billing alerts**
  - Configure billing alarms in CloudWatch
  - Set up AWS Budgets for cost control
  - Monitor unusual spending patterns
  - Review costs regularly

---

## 7. Incident Response

### 7.1 Incident Response Plan
- [ ] **Develop incident response procedures**
  - Create incident response playbooks
  - Define roles and responsibilities
  - Establish communication channels
  - Document escalation procedures

### 7.2 Emergency Access
- [ ] **Prepare for emergency scenarios**
  - Create emergency access procedures
  - Maintain offline access to critical information
  - Test emergency access procedures
  - Document break-glass procedures

---

## 8. Regular Maintenance and Auditing

### 8.1 Regular Reviews
- [ ] **Conduct monthly security reviews**
  - Review IAM users and permissions
  - Audit access keys and credentials
  - Check for unused resources
  - Review security group rules

- [ ] **Generate and review credential reports**
  - Download IAM credential reports monthly
  - Identify unused credentials
  - Check password and access key ages
  - Remove unnecessary credentials

### 8.2 Automated Compliance Checking
- [ ] **Implement automated security checks**
  - Use AWS Config rules for compliance monitoring
  - Set up Security Hub for continuous assessment
  - Configure automated remediation where appropriate
  - Regular vulnerability scanning

---

## 9. Documentation and Training

### 9.1 Documentation
- [ ] **Maintain security documentation**
  - Document security policies and procedures
  - Keep architecture diagrams updated
  - Maintain incident response playbooks
  - Document configuration changes

### 9.2 Training and Awareness
- [ ] **Provide security training**
  - Train team members on AWS security best practices
  - Conduct regular security awareness sessions
  - Keep team updated on new security features
  - Practice incident response procedures

---

## Implementation Priority

### Phase 1 (Critical - Implement Immediately)
1. Enable MFA on root account
2. Remove/secure root access keys
3. Enable CloudTrail in all regions
4. Set up basic IAM users with MFA
5. Configure security alerts

### Phase 2 (High Priority - Implement within 30 days)
1. Implement federated access
2. Enable GuardDuty and Security Hub
3. Configure AWS Config
4. Set up VPC Flow Logs
5. Implement encryption at rest

### Phase 3 (Medium Priority - Implement within 90 days)
1. Implement comprehensive monitoring
2. Set up automated compliance checking
3. Develop incident response procedures
4. Implement backup strategies
5. Conduct security training

---

## Verification and Testing

After implementing each control, verify its effectiveness by:

- [ ] Testing the control functionality
- [ ] Reviewing logs and monitoring data
- [ ] Conducting simulated security events
- [ ] Documenting test results
- [ ] Updating procedures based on findings

---

## Additional Resources

- [AWS Security Best Practices](https://aws.amazon.com/architecture/security-identity-compliance/)
- [AWS Well-Architected Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)
- [AWS Startup Security Baseline](https://docs.aws.amazon.com/prescriptive-guidance/latest/aws-startup-security-baseline/welcome.html)
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [CloudTrail Best Practices](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/best-practices-security.html)

---

**Note**: This checklist provides foundational security controls. Organizations with specific compliance requirements or advanced security needs should implement additional controls as appropriate for their environment and risk profile.
