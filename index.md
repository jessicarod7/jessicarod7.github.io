---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
author_profile: true
---

Hi, I'm Jessica!

This is my personal website, where I blog about life, tech, and everything else. There isn't a theme or set schedule to
what I put up, but I'm liking this "every two to three months" thing I have going on. Either way, you can be pleasantly
surprised by subscribing to the [RSS feed]({{ "/feed.xml" | absolute_url }}).

I'm also in a lot of places on the Internet: check the links in the sidebar or behind the follow button. 

My current GPG key is `6D71 C504 6A9A 4B98 DAE9 5EE8 BBA3 C03E D204 C805`, corresponding to _dev_ at _jessicarod_, dot com.
You can retrieve it via [WKD](https://wiki.gnupg.org/WKD), [keys.openpgp.org](https://keys.openpgp.org/) and
[keyserver.ubuntu.com](https://keyserver.ubuntu.com/). The easiest way to do it is via your email client (which likely
supports WKD), or in the terminal with [sq](https://sequoia-pgp.gitlab.io/user-documentation):

```shell
# Pull from WKD and the keyservers
sq network search <fingerprint_or_email>
# Trust the cert once you verify it
sq pki link add --cert <fingerprint> --all
```