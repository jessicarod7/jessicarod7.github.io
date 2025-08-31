---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
author_profile: true
---

Hi, I'm Jessica!

This is my personal website, where I blog about life, ideas, and everything else. There isn't a pattern to
when I post, but I'm liking this "every two to three months" thing I have going on. Either way, you can be pleasantly
surprised by subscribing to the [RSS feed]({{ "/feed.xml" | absolute_url }}).

I have profiles all over the Internet; check the links in the sidebar/follow button. I'm most active on Signal (and
increasingly IRL), so ask for my username via the other channels.

My current GPG key is `6D71 C504 6A9A 4B98 DAE9 5EE8 BBA3 C03E D204 C805`, corresponding to _dev_ at _jessicarod_, dot com.
You can retrieve it via [WKD](https://wiki.gnupg.org/WKD) and various keyservers. The easiest way to do that is via your email client, or in
the terminal with [sq](https://book.sequoia-pgp.org/):

```shell
# Pull from WKD and the keyservers
sq network search <fingerprint_or_email>
# Trust the cert once you verify it
sq pki link add --cert <fingerprint> --all
```