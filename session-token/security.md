# 🛡 Security



## Security Model

1\. The session keys are like your JWT Token adapted to web3

2\. These keys have an expiry and a scope

3\. Once the key has expired they can't be reused in the target program. A new session token is to be generated.

4\. They are also designed to be revokable, so that in the worst case if something wrong happens the attack surface is limited to the ephemeral keypair and the assets contained in them. i.e **0.01 SOL**

### Note on IndexedDB

Web Browser is an extremely adverserial environment, no amount of security is enough there, right from cookies to session to extension's sandbox.

Attackers could always inject arbitrary code via XSS or malicious extension. This is why users are discouraged from storing serious funds in a browser wallet, they are only for day to day expenses and it is really important to establish the distinction that **Session Keys are not burner wallets.**

However, majority of web3 today is on web browsers and that's how users primarily interact with other dApps. Given the constraints of today, we have to **design around them** and **harden** them via other means.

**Here's we approach it**

1. Drastically reduce the scope of what's possible with an ephemeral signer, they are highly context and use case specific.
2. This is similar to the approach to JWT in a typical client server architecture in web2.
3. &#x20;For example the session or JWT tokens on Facebook, twitter or even banking website could be vulnerable to the same issue. The way they address is this by **limiting the scope** of what **you can do with a token**, have **intelligent systems** in place to **revoke** them and further introduce 2FA for suspicious activity.
4. We follow a similar model to limit the scope, we are also working on adding intelligent revocation systems which can revoke a compromised token as soon as we witness a malicious transaction like out of scope usage etc. **The absolute worst case scenario in terms of loss of funds is the 0.01 SOL topped up to pay for the gas fee.**
5. Also**, developers can work around this today by pairing it with a gasless relay like octane and setting `topUp to false`**. However, we don't have a seamless way to do it directly from our SDK yet, although it is on our roadmap.

\
Audits
------

As of today, we are unaudited, primarily due to financial constraints imposed by the general market. However, we do plan to get it audited. In the meantime, all the code is open source, if you find what we have useful, find a vulnerability please follow our [security policy](https://github.com/gumhq/sdk/security/policy) and create a security advisory, we will respond to it as a top priority.
