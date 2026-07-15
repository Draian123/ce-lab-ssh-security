# Security Configuration Matrix

## Instance Details

- Instance ID: i-0a1b2c3d4e5f6g7h
- Instance Type: t3.micro
- Public IP: 54.123.45.67
- Private IP: 172.31.0.42
- Region: us-east-1
- AMI: Ubuntu 22.04 LTS

## Security Group: week2-web-server-sg

### Inbound Rules

| Protocol | Port | Source     | Purpose                  | Risk Level          |
| -------- | ---- | ---------- | ------------------------ | ------------------- |
| TCP      | 22   | YOUR_IP/32 | SSH admin access         | Low (restricted IP) |
| TCP      | 80   | 0.0.0.0/0  | Public HTTP web traffic  | Low (expected)      |
| TCP      | 443  | 0.0.0.0/0  | Public HTTPS web traffic | Low (expected)      |

### Outbound Rules

| Protocol | Port | Destination | Purpose             |
| -------- | ---- | ----------- | ------------------- |
| All      | All  | 0.0.0.0/0   | Unrestricted egress |

## Security Assessment

### ✅ Strengths

- SSH access restricted to single IP (YOUR_IP/32)
- No unnecessary ports exposed
- Web ports (80/443) open only to required traffic (0.0.0.0/0)
- t3.micro appropriate for learning environment

### ⚠️ Areas for Improvement

- **Outbound rules**: Currently unrestricted (0.0.0.0/0)—consider limiting to required destinations
- **HTTPS enforcement**: HTTP on port 80 should redirect to HTTPS
- **Logging**: VPC Flow Logs not enabled
- **Monitoring**: No CloudWatch alarms for suspicious activity

## Recommendations

### Priority 1 (Implement Now)

1. ✅ Keep SSH restricted to your IP/32
2. Add HTTPS (port 443) to security group
3. Enable VPC Flow Logs for traffic monitoring
4. Configure CloudWatch alarms for failed SSH attempts

### Priority 2 (Consider)

1. Restrict outbound rules to known destinations (port 443 for updates, DNS 53)
2. Use AWS Systems Manager Session Manager instead of SSH (no port 22 needed)
3. Enable AWS Config for security compliance tracking
4. Implement auto-patching for OS updates

### Priority 3 (Future/Production)

1. Use AWS WAF for DDoS protection on web ports
2. Implement GuardDuty for threat detection
3. Enable MFA for SSH access
4. Use bastion host pattern for SSH access (instead of direct IP exposure)

## Audit Checklist

- [ ] Confirmed no databases exposed to internet (0.0.0.0/0)
- [ ] SSH restricted to known IP addresses
- [ ] Web ports (80/443) restricted to appropriate protocols only
- [ ] Outbound rules reviewed and justified
- [ ] Security group named descriptively (week2-web-server-sg ✓)
- [ ] No overly permissive source ranges (e.g., 0.0.0.0/0 on SSH)

## Compliance Notes

- **CIS AWS Foundations**: Rule 4.1—Security groups should restrict ingress access ✅
- **AWS Well-Architected**: Follows principle of least privilege for SSH
- **Bootcamp Week 2 Scope**: Configuration appropriate for learning environment
