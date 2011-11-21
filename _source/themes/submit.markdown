---
layout: main.html
title: Submitting Fbsplash Themes
---

Submitting Fbsplash Themes
--------------------------

So, you created a cool new theme and you want to share it with the rest of the
world? The best way to do this is to submit it for inclusion in our theme
repository. To do so, just follow the guidelines below:

1. Make sure your theme contains a valid `metadata.xml` file. The metadata is
important, as it will be used to automatically generate info pages in our theme
repository.

2. Validate your theme. You can do this in a variety of different ways. First,
download [`thememeta.xsd`](/themes/files/thememeta.xsd) and then run:

    * using `libxml2` (`dev-libs/libxml2` in Gentoo): `xmllint –schema thememeta.xsd metadata.xml`,
    * using `xmlstarlet` (`app-text/xmlstarlet` in Gentoo): `xml val -e -s thememeta.xsd metadata.xml`.

3. Make sure your theme is properly packed. The theme package should be a
gzip-ed (`tar.gz`) or bzip2-ed (`tar.bz2`) tar archive containing a single
directory which should match the name of your theme as specified in the
`metadata.xml` file.

4. Send your theme to <alanhaggai@alanhaggai.org>.

5. Wait for it to be processed and to appear in the theme repository on this
website. Since theme submissions are processed by a human, it might take a few
days before the process is completed.
