# BitSight Security Remediation User Stories

## LPM-13457: Upgrade TLS Configuration for sso.lpm.intrado.com

**As a** security engineer  
**I want to** disable insecure TLS protocols (TLSv1.0, TLSv1.1) on sso.lpm.intrado.com  
**So that** the SSO service only accepts secure connections using modern TLS standards

This story addresses a critical security vulnerability where the SSO service currently allows insecure TLS protocols, exposing authentication flows to potential attacks.

### Technical Notes
* Finding ID: sso.lpm.intrado.com:443
* This affects authentication flows - thorough testing required
* Consider gradual rollout with monitoring

### Acceptance Criteria
* Current TLS configuration audited and documented
* TLSv1.0 and TLSv1.1 protocols disabled
* TLS 1.2 and TLS 1.3 enabled and properly configured
* Cipher suites updated to use only secure, modern ciphers
* SSO functionality tested with various clients to ensure compatibility
* Coordination completed with teams that integrate with SSO service
* All dependent systems continue to function properly

---

## LPM-13458: Implement Security Headers for sso.lpm.intrado.com

**As a** security engineer  
**I want to** implement proper HTTP security headers on sso.lpm.intrado.com  
**So that** the SSO service is protected against common web vulnerabilities like XSS, clickjacking, and MITM attacks

This story addresses missing and ineffective security headers that leave the SSO service vulnerable to web-based attacks, improving the overall security posture of the authentication system.

### Technical Notes
* Current issues: "Insecure configuration; Ineffective headers: Cache-Control, Set-Cookie; Missing required headers"
* SSO applications require careful CSP configuration to avoid breaking functionality
* Consider impact on embedded authentication flows

### Acceptance Criteria
* Content Security Policy (CSP) header implemented with appropriate directives
* Strict-Transport-Security header added for HSTS
* X-Frame-Options header implemented to prevent clickjacking
* X-Content-Type-Options header added to prevent MIME sniffing
* Referrer-Policy header implemented
* X-XSS-Protection header added (for legacy browser support)
* Ineffective Cache-Control and Set-Cookie headers fixed
* Headers tested using security scanning tools
* No functionality regression in SSO flows

---

## LPM-13459: Implement Security Headers for intrado-copilot-metrics.lpm.intrado.com

**As a** security engineer  
**I want to** implement HTTP security headers on intrado-copilot-metrics.lpm.intrado.com  
**So that** the metrics application is protected against common web vulnerabilities

This story addresses the complete absence of security headers on the Copilot metrics application, bringing it up to security standards and reducing potential attack vectors.

### Technical Notes
* Finding ID: intrado-copilot-metrics.lpm.intrado.com:443
* Current issues: "No security headers are set"
* Metrics applications often have specific CSP requirements for dashboards and data visualization
* May need to accommodate third-party libraries and widgets

### Acceptance Criteria
* Content Security Policy (CSP) header implemented appropriate for metrics dashboard
* Strict-Transport-Security header added for HSTS
* X-Frame-Options header implemented
* X-Content-Type-Options header added
* Referrer-Policy header implemented
* X-XSS-Protection header added
* Headers tested and metrics functionality verified as not impacted

---

## LPM-13460: Implement SSL/TLS Certificate Monitoring with New Relic Synthetics

**As a** platform engineer  
**I want to** implement automated certificate monitoring for all LPM SSL certificates using New Relic Synthetics  
**So that** we can proactively prevent certificate expirations and maintain secure configurations

This story establishes proactive monitoring to prevent future certificate-related security findings and service disruptions across the LPM platform.

### Technical Notes
* Use New Relic Synthetics SSL certificate monitoring (https://support.newrelic.com/s/hubtopic/aAX8W0000008ZtLWAU/relic-solution-monitoring-ssl-certificate-expiration)
* All services run in Azure - consider Azure Key Vault for certificate management
* This is a preventative measure to avoid future security vulnerabilities
* Should cover all LPM subdomains: sso.lpm, intrado-copilot-metrics.lpm
* Consider Azure App Service certificate auto-renewal for applicable services

### Acceptance Criteria
* All LPM-related SSL certificates inventoried with expiration dates documented
* New Relic Synthetics SSL certificate monitoring set up for each LPM domain
* Certificate expiration alerts configured for 30, 14, and 7 days before expiration
* Azure Key Vault integration implemented for automated certificate renewal where possible
* Alerts integrated with existing notification channels (email, Slack, PagerDuty)
* Certificate renewal procedures documented for manual certificates
* Runbooks created for certificate renewal processes in Azure
* Certificate status integrated into existing New Relic dashboards


