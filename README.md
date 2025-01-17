
<!-- This document must be rendered in RStudio using the option "knitr with parameters" or rmarkdown::render("MyDocument.Rmd", params = list(password = "my_password"))-->
<!-- README.md is generated from README.Rmd. Please edit .Rmd file -->

# mRpostman <img src="man/figures/logo.png" align="right" width="140" />

<!-- # mRpostman <img src="man/figures/logo.png" align="right" /> -->
<!-- [![Downloads](http://cranlogs.r-pkg.org/badges/mRpostman?color=brightgreen)](http://www.r-pkg.org/pkg/mRpostman) -->
<!-- one space after links to display badges side by side -->

[![Travis-CI Build
Status](https://travis-ci.org/allanvc/mRpostman.svg?branch=master)](https://travis-ci.org/allanvc/mRpostman)
[![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/mRpostman)](https://cran.r-project.org/package=mRpostman)
[![Downloads from the RStudio CRAN
mirror](https://cranlogs.r-pkg.org/badges/mRpostman)](https://cran.r-project.org/package=mRpostman)
[![CRAN/METACRAN](https://img.shields.io/cran/l/mRpostman)](https://opensource.org/licenses/GPL-3.0)

An IMAP Client for R

## Overview

`mRpostman` is an easy-to-use IMAP client that provides tools for
message searching, selective fetching of message attributes, mailbox
management, attachment extraction, and several other IMAP features. The
aim of this package is to pave the way for email data analysis in R. To
do so, `mRpostman` makes extensive use of the {curl} package and the
libcurl C library.

mRpostman’s official website: <https://allanvc.github.io/mRpostman/>

**IMPORTANT**:

1.  In version `0.9.0.0`, `mRpostman` went trough substantial changes,
    including ones that have no backward compatibility with versions
    `<= 0.3.1`. A detailed vignette on how to migrate your mRpostman’s
    deprecated code to the new syntax is available at [*“Migrating old
    code to the new mRpostman’s
    syntax”*](https://allanvc.github.io/mRpostman/articles/code_migration.html).

2.  Old versions of the libcurl C library ({curl}’s main engine) will
    cause the malfunction of this package. If your libcurl’s version is
    above 7.58.0, you should be fine. In case you intend to use OAuth
    2.0 authentication, then you will need libcurl &gt;= 7.65.0. To
    learn more about the OAuth 2.0 authentication in this package, refer
    to the [*“Using IMAP OAuth2.0 authentication in
    mRpostman”*](https://allanvc.github.io/mRpostman/articles/xoauth2.0.html)
    vignette.

## Authentication

There are two ways of connecting to your IMAP server: using plain or
OAuth2.0 authentication. Here, we only describe the plain authentication
process. If you want to use OAuth2.0 authentication, please read the
aforementioned vignette.

### Allowing less secure apps access

When using plain authentication, most of the mail providers will require
the user to enable **less secure apps** access. Once it is done, you
will be able to access your mailbox using a “third party app” as
`mRpostman`.

### Plain authentication

Before using `mRpostman`, it is essential to configure the access to
your email account. Various mail providers require that you enable
**“less secure apps”** access to accept plain authentication between the
IMAP server and a third-party app.

Let’s see how to configure simple plain authentication for Gmail, Yahoo
Mail, AOL Mail, Hotmail, and Office 365.

#### Outlook - Office 365

There is no need to execute any external configuration. Please, note
that the `url` parameter in `configure_imap()` should be set as
`url = "imaps://outlook.office365.com"`, and the `username` should be
set as `user@yourcompany.com`.

#### Hotmail

There is no need to execute any external configuration. Please, note
that the `url` parameter in `configure_imap()` should be set as
`url = "imaps://imap-mail.outlook.com"`, and the `username` should be
set as `user@hotmail.com`.

#### Gmail

1.  Go to the Gmail website and log in with your credentials.

2.  Then, go to
    <https://myaccount.google.com/u/1/lesssecureapps?pageId=none>

![](man/figures/gmail1.png) <!-- <img src="man/figures/gmail1.png"> -->

3.  Set “Allow less secure apps” to **ON**.

#### Yahoo Mail

1.  Go to the Yahoo Mail website and log in with your credentials.

2.  Click on “Account Info”.

![](man/figures/yahoo1.png) <!-- <img src="man/figures/yahoo1.png"> -->

3.  Click on “Account Security” on the left menu.

![](man/figures/yahoo2.png) <!-- <img src="man/figures/yahoo2.png"> -->

4.  Then, set “Allow apps that use less secure sign in” **ON**

![](man/figures/yahoo3.png)

<!-- <img src="man/figures/yahoo3.png"> -->

#### AOL Mail

1.  Go to the AOL Mail website and log in with your credentials.

2.  Click on “Options” and then on “Account Info”.

![](man/figures/aol1.png) <!-- <img src="man/figures/aol1.png"> -->

3.  Click on “Account Security” on the left menu.

![](man/figures/aol2.png) <!-- <img src="man/figures/aol2.png"> -->

4.  After, set “Allow apps that use less secure sign in” **ON**

![](man/figures/aol3.png) <!-- <img src="man/figures/aol3.png"> -->

## Introduction

From version 0.9.0.0 onward, `mRpostman` is implemented under the OO
paradigm, based on an R6 class called `ImapCon`. Its derived methods,
and a few independent functions enable the R user to perform a myriad of
IMAP commands.

The package is divided in 8 groups of operations. Below, we present all
the available methods and functions:

-   **configuration methods**: `configure_imap()`, `reset_url()`,
    `reset_username()`, `reset_password()`, `reset_verbose()`,
    `reset_use_ssl()`, `reset_buffersize()`, `reset_timeout_ms()`,
    `reset_xoauth2_bearer()`;
-   **server capabilities method**: `list_server_capabilities()`;
-   **mailbox operations methods**: `list_mail_folders()`,
    `select_folder()`, `examine_folder()`, `rename_folder()`,
    `create_folder()`, `list_flags()`;
-   **single-search methods**: `search_before()`, `search_since()`,
    `search_period()`, `search_on()`,
    `search_sent_before()`,`search_sent_since()`,
    `search_sent_period()`, `search_sent_on()`, `search_string()`,
    `search_flag()`, `search_smaller_than()`, `search_larger_than()`,
    `search_younger_than()`, `search_older_than()`;
-   **the custom-search method and its helper functions**: `search()`;
    -   relational operators functions: `AND()`, `OR()`;
    -   criteria definition functions: `before()`, `since()`, `on()`,
        `sent_before()`, `sent_since()`, `sent_on()`, `string()`,
        `flag()`, `smaller_than()`, `larger_than()`, `younger_than()`,
        `older_than()`;
-   **fetch methods**: `fetch_body()`, `fetch_header()`, `fetch_text()`,
    `fetch_metadata()`, `fetch_attachments_list()`,
    `fetch_attachments()`;
-   **attachments methods**: `list_attachments()`, `get_attachments()`,
    `fetch_attachments_list()`, `fetch_attachments()`;
-   **complementary methods**: `copy_msg()`, `move_msg()`,
    `esearch_min_id()`, `esearch_max_id()`, `esearch_count_msg()`,
    `delete_msg()`, `expunge()`, `add_flags()`, `remove_flags()`,
    `replace_flags()`.

## Installation

``` r
# CRAN version
install.packages("mRpostman")

# Dev version
if (!require('remotes')) install.packages('remotes')
remotes::install_github("allanvc/mRpostman")
```

## Basic Usage

### 1) Configure an IMAP connection and list the server’s capabilities

``` r
library(mRpostman)

# Outlook - Office 365
con <- configure_imap(url="imaps://outlook.office365.com",
                      username="your_user@company.com",
                      password=rstudioapi::askForPassword()
)

# other IMAP providers that were tested: Hotmail ("imaps://imap-mail.outlook.com"),
#  Gmail (imaps://imap.gmail.com), Yahoo (imaps://imap.mail.yahoo.com/), 
#  AOL (imaps://export.imap.aol.com/), Yandex (imaps://imap.yandex.com)

# Other non-tested mail providers should work as well

con$list_server_capabilities()
```

### 2) List mail folders and select “INBOX”

``` r
# Listing
con$list_mail_folders()

# Selecting
con$select_folder(name = "INBOX")
```

### 3) Search messages by date

``` r
res1 <- con$search_on(date_char = "02-Jan-2020")

res1
```

### 4) Customizing a search with multiple criteria

Executing a search by string:

``` r
# messages that contain either "@k-state.edu" OR "ksu.edu" in the "TO" header field
res2 <- con$search(OR(
  string(expr = "@k-state.edu", where = "TO"),
  string(expr = "@ksu.edu", where = "TO")
))

res2
```

### 5) Fetch messages’ text using single-search results

``` r
res3 <- con$search_string(expr = "Welcome!", where = "SUBJECT") %>%
  con$fetch_text(write_to_disk = TRUE) # also writes results to disk

res3
```

### 6) Attachments

You can list the attachments of one or more messages with:

1.  the `list_attachments()` function:

``` r
con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_text() %>% # or with fetch_body()
  list_attachments() # does not depend on the 'con' object
```

… or more directly with:

2.  `fetch_attachments_list()`

``` r
con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_attachments_list()
```

If you want to download the attachments of one or more messages, there
are also two ways of doing that.

1.  Using the `get_attachments()` method:

``` r
con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_text() %>% # or with fetch_body()
  con$get_attachments()
```

… and more directly with the

2.  `fetch_attachments()` method:

``` r
con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_attachments()
```

## Future Improvements

-   add further IMAP features;
-   eliminate the {stringr} dependency in REGEX;
-   implement a progress bar in fetch operations;

## Known bugs

-   *search results truncation*: This is a [libcurl’s known
    bug](https://curl.se/docs/knownbugs.html#IMAP_SEARCH_ALL_truncated_respon)
    which causes the search results to be truncated when there is a
    large number of message ids returned. To circumvent this problem,
    you can set a higher `buffersize` value, increasing the buffer
    capacity, and `verbose = TRUE` for monitoring the server response
    for truncated results when executing a search. When possible,
    `mRpostman` tries to issue a warning for possible truncated values.

-   *`verbose = TRUE` malfunction on Windows*: This seems to be related
    to the [{curl} R
    package](https://github.com/jeroen/curl/issues/230). When using the
    `verbose = TRUE` on Windows, the flow of information between the
    IMAP server and the R session presents an intermittent behavior,
    which causes it to not be shown on the console, or with a
    considerable delay.

-   *shared mailbox access not working*: This seems to be another
    [libcurl’s bug](https://github.com/allanvc/mRpostman/issues/2),
    although more tests need to be done to confirm it. It does not allow
    the user to connect to a shared mailbox. To circumvent this, if the
    shared mailbox has a password associated with it, you can try a
    direct regular connection.

-   *`xoauth2_bearer` SASL error*: This is related to [old libcurl’s
    versions](https://curl.se/bug/?i=2487) which causes the access token
    to not be properly passed to the server. This bug was fixed in
    libcurl 7.65.0. The problem is that many Linux distributions, such
    as Ubuntu 18.04, still provide libcurl 7.58.0 in their official
    distribution (libcurl4-openssl-dev). If you use a newer Linux distro
    such as Ubuntu 20.04, you should be fine as the distributed
    libcurl’s version will be above 7.65.0. Another alternative is to
    use plain authentication instead of OAuth2.0.

## License

This package is licensed under the terms of the GPL-3 License.

## References

Crispin, M. (2003), *INTERNET MESSAGE ACCESS PROTOCOL - VERSION 4rev1*,
RFC 3501, March 2003, <https://www.rfc-editor.org/rfc/rfc3501>.

Heinlein, P. and Hartleben, P. (2008). *The Book of IMAP: Building a
Mail Server with Courier and Cyrus*. No Starch Press. ISBN
978-1-59327-177-0.

Ooms, J. (2020), *curl: A Modern and Flexible Web Client for R*. R
package version 4.3, <https://CRAN.R-project.org/package=curl>

Stenberg, D. *Libcurl - The Multiprotocol File Transfer Library*,
<https://curl.se/libcurl/>
