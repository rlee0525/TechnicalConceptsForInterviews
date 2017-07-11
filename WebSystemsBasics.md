# Web Systems Basics

### HTTP vs. HTTPS
- HTTPS is the secure version of HTTP, the protocol over which data is sent between your browser and the website that you are connected to.
- S stands for 'Secure' and typically use SSL (Secure Sockets Layer) or TLS (Transport Layer Security) to encyrpt all communications between your browser and the website encrypted.
- TLS and SSL both use 'asymmetric' Public Key Infrastructure (PKI) system, which uses a public key and a private key to encrypt communications. Anything encrypted with the public key can only be decrypted by the private key and vice-versa.
- HTTPS is often used to protect highly confidential online transactions like online banking and online shopping order forms.

### XSS vs. CSRF
- CSRF (Cross-Site Request Forgery) attack - the attacker tries to force/trick you into making a request which you did not intend. 
  
    e.g. Sending you a link that makes you involuntarily change your password. 

    ```https://security.stackexchange.com/account?new_password=abc123```

- XSS (Cross-Site Scripting) attack - the attacker makes you involuntarily execute client-side code, most likely JS. 

  e.g. ```https://security.stackexchange.com/search?q="><script>alert(document.cookie)</script>```

- Both attacks have in common that they are client-side attacks and need some form of user activity.
- Unlike RFI (Remote File Inclusion) or SQLi (SQL injection), you are attacking a user rather than the server.
- XSS is generally more powerful because it allows the execution of arbitrary script code while CSRF is restrcted to a particular action.
- XSS attack also effectively bypasses all anti-CSRF measures if conducted successfully.

### Cookie / SessionStorage / LocalStorage
- localStorage and sessionStorage are both WebStorages and features of HTML5.
- localStorage stores information as long as the user does not delete them.
- sessionStorage stores information as long as the session goes. (Typically until the user closes the tab/browser).
- cookies are supported by older browsers (pre HTML5) and usually are a fallback for frameworks
- cookies can store way less information than WebStorages
- WebStorage information is never transferred to the server.

