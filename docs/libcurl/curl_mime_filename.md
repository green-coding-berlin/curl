---
c: Copyright (C) Daniel Stenberg, <daniel.se>, et al.
SPDX-License-Identifier: curl
Title: curl_mime_filename
Section: 3
Source: libcurl
See-also:
  - curl_mime_addpart (3)
  - curl_mime_data (3)
  - curl_mime_filedata (3)
---

# NAME

curl_mime_filename - set a mime part's remote file name

# SYNOPSIS

~~~c
#include <curl/curl.h>

CURLcode curl_mime_filename(curl_mimepart *part,
                            const char *filename);
~~~

# DESCRIPTION

curl_mime_filename(3) sets a mime part's remote file name. When remote
file name is set, content data is processed as a file, whatever is the part's
content source. A part's remote file name is transmitted to the server in the
associated Content-Disposition generated header.

*part* is the part's handle to assign the remote file name to.

*filename* points to the null-terminated file name string; it may be set
to NULL to remove a previously attached remote file name.

The remote file name string is copied into the part, thus the associated
storage may safely be released or reused after call. Setting a part's file
name multiple times is valid: only the value set by the last call is retained.

# EXAMPLE

~~~c

static char imagebuf[]="imagedata";

int main(void)
{
  curl_mime *mime;
  curl_mimepart *part;

  CURL *curl = curl_easy_init();
  if(curl) {
    /* create a mime handle */
    mime = curl_mime_init(curl);

    /* add a part */
    part = curl_mime_addpart(mime);

    /* send image data from memory */
    curl_mime_data(part, imagebuf, sizeof(imagebuf));

    /* set a file name to make it look like a file upload */
    curl_mime_filename(part, "image.png");

    /* set name */
    curl_mime_name(part, "data");
  }
}
~~~

# AVAILABILITY

As long as at least one of HTTP, SMTP or IMAP is enabled. Added in 7.56.0.

# RETURN VALUE

CURLE_OK or a CURL error code upon failure.
