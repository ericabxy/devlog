---
layout: post
title:  "0install - Anarch and the Executable Bit"
date:   2022-05-18 17:15:00 -0800
categories: 0install, packaging
---

0install - Anarch and the Executable Bit
-----------------------------

Several years ago I began learning to package software for computers
using the Zero Install (0install) installation system. This process
involves learning some things about dependencies and linking, as
well as controlling a handful of packaging and installation programs.
0install uses XML files that live on the internet and tell 0install how
to download, setup, and run publically available software automatically
for the user.

I recently returned to the 0install system to learn more about
distributing software and contributing to the project. My goals are to
cooperate with the developers and community to package software and
troubleshoot problems, writing explanations and tutorials along the
way. This post is the first in a series.

Packaging Anarch HD
===================

Anarch is a small video game designed to be trivially easy to compile
and run. Binaries are provided for every release. Older releases are
deleted by the author, meaning versions could become unavailable in the
future, but for now the latest official releases are good candidates
for a 0install "feed" because Anarch boasts of not requiring any
dependencies. Everything needed is encapsulated in a single binary
file.

I chose to use [0template][1] to assist me in writing releases for
Anarch. The [template file][2] I wrote with 0template resides in my Zero
Install repository on GitLab. The structure of this template groups
releases under the same license. Each release will be contained in its
own "implementation", where 0template will replace {version} with the
version number and put a verification hash for the download in
"manifest-digest".

Notably, Anarch is not hosted in archive format. Each release is a
single executable file. Thus, instead of the "archive" tag this
template must use the "file" tag and use the "executable" attribute to
set the executable bit. I don't know if the "dest" attribute is
necessary for this to work.

A Problem
=========

After using 0template to generate an implementation and test the feed
locally, it turns out the executable bit was not set automatically!
The game now can't run without the user first manually setting the bit.
This is bad for the user experience, but in addition this means the
downloaded file--once set to executable--no longer matches the
generated verification hash!

I joined an posted to the zero-install-bugs mailing list and was
informed that my current version of 0template uses an older version
of 0install that doesn't support the executable bit. This means that
although 0install can officially download and run this program the way
I intended, 0template can't properly assist in generating the XML file.
However, I can manually edit the XML to use the correct hash despite
this.

Updating the "sha256new" attribute of the "manifest-digest" tag with
the actual hash of the file--the hash generated once 0install downloads
the file and sets the executable bit--works because 0template only
mistakenly generated the wrong hash because it didn't set the
executable bit first. Editing the hash manually effectively corrects
the mistake 0template made.

Follow Up
=========

After correcting the hash, I updated the previously-not-working feed
with the new one, and notified the mailing list that the fix was
successful and invited anyone to test the [new feed][3].

Since the behaviour of 0template in this case is a serious bug, I
inquired at the mailing list how the bug can be corrected (in case
I can contribute to the project on this matter).

Since 0install is a decentralized and distributed install system, it
may be useful to take measures to future-proof this feed. In the case
of Anarch, the developer takes down older versions whenever there is an
update. The releases are hosted at GitLab. To counter-act a
disappearing release version, I could link the feed to older versions
by taking advantage of GitLab's history browser. However, a more
elegant solution may be to save all current and past versions of
Anarch for posterity, upload them to Internet Archive, and link the
feed to those versions. Additionally, since the binaries are small, it
may be fine to simply host releases myself at some webspace; perhaps
even where the feed itself resides!

[1]: https://docs.0install.net/tools/0template/
[2]: https://gitlab.com/ericxdu/feeds/-/blob/master/templates/Anarch.xml.template
[3]: https://ericxdu.gitlab.io/feeds/Anarch.xml
