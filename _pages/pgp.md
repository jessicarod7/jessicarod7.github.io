---
title: My PGP public key
permalink: /pgp
---
I use my PGP key to sign commits and tags on a limited number of personal projects. While I will respond to emails sent
using PGP, I'd strongly recommend against doing so:

- Various issues best explained by experts. Like [this episode of Security Cryptography
  Whatever](https://securitycryptographywhatever.com/2025/08/22/stop-using-encrypted-email-with-william-woodruff/), or
  [this blog by Soatok](https://soatok.blog/2024/11/15/what-to-use-instead-of-pgp/).
- I'm not a regular user of PGP+email and will probably screw up somewhere.
- I've been down this road before and it only leads to pain and suffering. Let alone getting things to work nicely on
  mobile.
  - I'll only check from my PCs for this reason.

If you need secure communications, ask for my Signal username. Otherwise, we can try to work something out but no
guarantees.

My current PGP key is `81C7 F5CA ECED D144 FB95 B160 B2DA E4B4 2250 7308`, corresponding to
<span style="white-space:nowrap;"><code>[dev at jessicarod, dot com]</code></span> and <span style="white-space:nowrap;"><code>[hi at the-same-domain]</code></span>. Retrieve it from
[WKD](https://datatracker.ietf.org/doc/draft-koch-openpgp-webkey-service/) and keyservers using the [sq CLI](https://book.sequoia-pgp.org/):

```shell
# Pull from WKD and the keyservers
sq network search <fingerprint_or_email>
# Trust the cert once you verify it
sq pki link add --cert <fingerprint> --all
```