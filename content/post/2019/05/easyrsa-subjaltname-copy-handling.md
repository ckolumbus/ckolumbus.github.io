---
title: "Easyrsa SubjAltName Copy Handling when signing"
date: 2019-05-05T16:34:45+02:00
draft: true
---

* easyrsa does not automatically copy SubjAltNames from CSRs 

* `--copy-ext` should(?) do the job?
* does'nt
```
    ERROR: adding extensions in section default
    140152253354496:error:22097082:X509 V3 routines:do_ext_nconf:unknown extension name:crypto/x509v3/v3_conf.c:78:
    140152253354496:error:22098080:X509 V3 routines:X509V3_EXT_nconf:error in extension:crypto/x509v3/v3_conf.c:47:name=copy_extensions, value=copy
```

* `copy_extention = copy` only works for `openssl ca` not for `sign` 
  (https://stackoverflow.com/questions/33989190/subject-alternative-name-is-not-copied-to-signed-certificate)


* Next Attempt: try to create extenstion with CA
* Alternative solution: pathc easyrsa to copy SubjAltName extentsions manually
  (similar to `renew` command)

