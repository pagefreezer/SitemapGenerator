SitemapGenerator
================

### Background

SitemapGenerator was built on Googles Sitemap Gen (http://goog-sitemapgen.sourceforge.net/) provided under the terms of the BSD license.
This version contains some enhencements, better documentation and examples.

### HOW TO RUN

Required python version 2.2 or later.
You can define filter rules and access log paths in `config.xml`.

Run by using this command:

    python sitemap_gen.py --config=config.xml

### CONFIGURATION
- **BASE URL**: the website's base url is set in `base_url="..."`. The base url must
  contain url protocol (`http://` or `https://`).

  Example:

```
    <site base_url="https://www.example-site.com/" ...
```

- **SITEMAP PATH**: Output file "sitemap.xml" will be generated in the
  `store_into="..."` path. This path can be relative or absolute.

  Some examples:

```
    ... store_into="./sitemap.xml"
    ... store_into="/var/www/sitemap.xml"
    ... store_into="../../htdocs/sitemap.xml"
```
- **ACCESS LOG PATH**: Access log path can be relative or absolute. It also
  supports basic wildcards to select multiple files. See examples.

  Support for:

  - Common Logfile Format (Apache)
  - Extended Logfile Format (IIS)
  - both plain text and gzipped text file

  Examples:

```
    <!-- One log file -->
    <accesslog path="/path-to-dir/access.log" ... />

    <!-- Directory's all log files select by extension
         This can found any txt file from dir -->
    <accesslog path="/path-to-dir/*.txt" ... />

    <!-- Like previous example. But more tight selection -->
    <accesslog path="/path-to-dir/access_log.*.txt" ... />

    <!-- Suitable for default configuration

        Can find these files

        /etc/httpd/logs/access.log
        /etc/httpd/logs/access.log.0
        /etc/httpd/logs/access.log.1.gz

        -->
    <accesslog path="/etc/httpd/logs/access.log*" ... />
```

### EXAMPLE FILTERS

> Single `?` character means one (1) character.
> Single `*` character means zero or any combination of multiple characters.

- `<filter action="drop" type="wildcard" pattern="//*" />`

  This filter will exclude the following urls:

```
    //
    //any-text
    ///
    //////
    //////any-text
```

- `<filter action="drop" type="wildcard" pattern="*/reaction.jsp*" />`

  This filter will exclude the following urls:

```
    /reaction.jsp
    /reaction.jsp?
    /reaction.jsp?foo=bar
    /any-prefix/reaction.jsp
    /any-prefix/reaction.jsp?
    /any-prefix/reaction.jsp?foo=bar
    /prefix/again/reaction.jsp
    /prefix/again/reaction.jsp?
    /prefix/again/reaction.jsp?foo=bar
```

- `<filter action="drop" type="wildcard" pattern="/[?]*" />`
  `[?]` is mean plain `?` character.

  This filter will exclude the following urls:
```
    /?
    /?foo
    /?foo=bar
    /?foo=bar&etc...
```
- `<filter action="drop" type="wildcard" pattern="*.gif" />`

  This filter will exclude the following urls:

```
    /path/to/images/lorem.gif
    /path/to/lorem.gif
    /path/to/etc.gif
```

- `<filter action="drop" type="wildcard" pattern="*.gif[?]*" />`

  This filter will exclude the following urls:

```
    /path/to/images/lorem.gif?
    /path/to/lorem.gif?123
    /path/to/etc.gif?foo=bar
```

  But won't exclude these urls due to the [?]:

```
    /path/to/images/lorem.gif
    /path/to/lorem.gif
    /path/to/etc.gif
```

