
# CVE References — Stored XSS via SVG Attachment Preview in ntfy

## Vulnerability Title
Stored Cross-Site Scripting (XSS) via SVG Attachment Preview

## Product
- Product: ntfy
- Vendor: binwiederhier
- Repository: https://github.com/binwiederhier/ntfy

## Advisory
- GitHub Security Advisory: GHSA-j8hr-p342-xrmh

## Vulnerability Type
- CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')

## Severity
- High

## CVSS v3.1
- Vector: `CVSS:3.1/AV:N/AC:L/PR:L/UI:R/S:C/C:L/I:H/A:N`
- Score: 8.0

## Affected Version
- ntfy v2.22.0

## Patched
- Patched via commit: `2911340`
- Fixed in upcoming release after advisory disclosure

## Summary
A stored cross-site scripting (XSS) vulnerability existed in the attachment preview functionality of ntfy. Attackers could upload a crafted SVG file containing embedded JavaScript payloads. When another user opened or previewed the malicious attachment, arbitrary JavaScript executed within the application's origin context.

This allowed attackers to perform authenticated actions on behalf of victims and potentially compromise administrator accounts.

## Technical Details

The application allowed SVG attachments to be rendered directly within the browser without proper sanitization or content restrictions.

Because SVG files support embedded JavaScript and event handlers, an attacker could upload a malicious SVG payload such as:

```svg
<svg xmlns="http://www.w3.org/2000/svg" onload="alert(document.domain)">
</svg>
````

When the attachment preview functionality rendered the SVG in-browser, the payload executed automatically.

## Steps to Reproduce

1. Create a malicious SVG file containing JavaScript payloads.
2. Upload the SVG file using the attachment upload functionality.
3. Access the uploaded attachment through the preview/open feature.
4. Observe that JavaScript executes in the victim's browser context.

## Impact

Successful exploitation could lead to:

* Session hijacking
* Authenticated request forgery
* Administrative account compromise
* Internal API abuse
* Sensitive data exfiltration

## Proof of Concept

### Payload

```svg
<svg xmlns="http://www.w3.org/2000/svg">
  <script>
    alert("xss")
  </script>
</svg>
```

## POC:
<img width="1916" height="982" alt="image" src="https://github.com/user-attachments/assets/eb55bec0-63d1-44ea-a2f1-4164a065f336" />
<img width="1919" height="982" alt="image" src="https://github.com/user-attachments/assets/456e9384-28e6-426d-a4e9-179c318f5cf0" />
<img width="1920" height="990" alt="image" src="https://github.com/user-attachments/assets/93aa766f-8bbb-4f47-8454-d37d32f8920a" />


## Patch Information

The issue was fixed by restricting unsafe SVG execution behavior in attachment previews.

Patch commit:

* [https://github.com/binwiederhier/ntfy/commit/2911340](https://github.com/binwiederhier/ntfy/commit/2911340)

## Timeline

| Date | Event                                 |
| ---- | ------------------------------------- |
| 2026 | Vulnerability discovered and reported |
| 2026 | Vendor acknowledged issue             |
| 2026 | Patch released                        |
| 2026 | Researcher confirmed fix              |

## Credits

Reported by:

* Venukamatchi (@Venukamatchi)

## References

* Advisory:

  * [https://github.com/binwiederhier/ntfy/security/advisories/GHSA-j8hr-p342-xrmh](https://github.com/binwiederhier/ntfy/security/advisories/GHSA-j8hr-p342-xrmh)

* Patch Commit:

  * [https://github.com/binwiederhier/ntfy/commit/2911340](https://github.com/binwiederhier/ntfy/commit/2911340)

* CWE-79:

  * [https://cwe.mitre.org/data/definitions/79.html](https://cwe.mitre.org/data/definitions/79.html)

* OWASP XSS:

  * [https://owasp.org/www-community/attacks/xss/](https://owasp.org/www-community/attacks/xss/)

## Disclosure Status

* Vendor acknowledged
* Patched
* Public advisory available


