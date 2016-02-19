Traverse Email Documentation v1.0:
----------------------------------

Contents
--------

  * [Overview](#overview)
  * [Pixel Block](#pixel-block)
  * [Domain](#domain)
  * [Identifiers](#identifiers)
  * [Example](#example)
  * [Best practices](#best-practices)

Overview
--------

To use Traverse in email, include the [Pixel Block](pixel-block) in your messages.

Please see the [example](#example) and [best practices](#best-practices), below.

Pixel Block
-----------

The Pixel Block is a series of generic pixels that redirect to our match partners' pixels when loaded. This means you don't need to update your integration when partners are added, removed, or change their pixels. We recommend including at least 10 pixels (`index`=0, 1, 2, ..., 9).

Each generic pixel has the following form:
 
> \<img style="border: 0px;" src="http://`domain`/pixel.gif?index=`index`&clientId=`clientId`&`identifiers`"/\>

| Parameter    | Description | Required |
| ------------ |------------ | -------- |
| `domain` | See [Domain](#domain), below. | Yes |
| `index` | Which partner's pixel to serve. | Yes |
| `clientId` | Your 36-character client ID (includes hyphens). | Yes |
| `identifiers` | See [Identifiers](#identifiers), below. | No |

Domain
------

Our pixels are hosted at `email.traversedlp.com`. However, in order to protect your deliverability, you should serve them from a domain you control, via a [CNAME record](https://en.wikipedia.org/wiki/CNAME_record).

For example, if your sending domain is `example.com`, you could create a CNAME record for `traverse.example.com` that resolves to `email.traversedlp.com`, and then use that subdomain in the [Pixel Block](#pixel-block).

Identifiers
-----------

Our match partners compute hashes using varying schemes. Most want the email address converted to lowercase before hashing, but some want it hashed via MD5 while others want SHA-1.

The following `identifiers` are supported:

| Parameter    | Description | *Recommended* |
| ------------ |------------ | ------------- |
| `emailMd5Lower` | MD5 hash of lowercase email address. | Yes |
| `emailSha1Lower` | SHA1 hash of lowercase email address. | Yes |
| `emailMd5Upper` | MD5 hash of *uppercase* email address. | No |
| `emailSha1Upper` | SHA1 hash of *uppercase* email address. | No |

For example, if the email address is `foo@BAR.com`, then `emailMd5Upper` would be `6d881a14e8fb4886b2742f0b4aa10d30`.

To accommodate the greatest number of partners, you would include all four `identifiers` in each pixel. However, at minimum, please at least include just `emailMd5Lower` for half of your pixels, and `emailSha1Lower` for the other.

Example
-------

Suppose you've set up a [CNAME record](#domain) pointing `traverse.example.com` to `email.traversedlp.com`, and you're sending an email to `foo@BAR.com`.

Include at least 5 pixels with `emailMd5Lower`, and at least 5 more with `emailSha1Lower`, like so (notice the changing `index`):

```
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=0&clientId=YOUR-CLIENT-ID-HERE&emailMd5Lower=f3ada405ce890b6f8204094deb12d8a8"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=1&clientId=YOUR-CLIENT-ID-HERE&emailMd5Lower=f3ada405ce890b6f8204094deb12d8a8"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=2&clientId=YOUR-CLIENT-ID-HERE&emailMd5Lower=f3ada405ce890b6f8204094deb12d8a8"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=3&clientId=YOUR-CLIENT-ID-HERE&emailMd5Lower=f3ada405ce890b6f8204094deb12d8a8"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=4&clientId=YOUR-CLIENT-ID-HERE&emailMd5Lower=f3ada405ce890b6f8204094deb12d8a8"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=5&clientId=YOUR-CLIENT-ID-HERE&emailSha1Lower=823776525776c8f23a87176c59d25759da7a52c4"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=6&clientId=YOUR-CLIENT-ID-HERE&emailSha1Lower=823776525776c8f23a87176c59d25759da7a52c4"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=7&clientId=YOUR-CLIENT-ID-HERE&emailSha1Lower=823776525776c8f23a87176c59d25759da7a52c4"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=8&clientId=YOUR-CLIENT-ID-HERE&emailSha1Lower=823776525776c8f23a87176c59d25759da7a52c4"/>
<img style="border: 0px;" src="http://traverse.example.com/pixel.gif?index=9&clientId=YOUR-CLIENT-ID-HERE&emailSha1Lower=823776525776c8f23a87176c59d25759da7a52c4"/>
```

Best practices
--------------

1. Always serve the [pixels](#pixel-block) from [a domain you control](#domain).
2. Only include the [pixels](#pixel-block) in HTML emails.
3. Always include at least 10 [pixels](#pixel-block): 5 with at least `emailMd5Lower` [`identifiers`](#identifiers), and 5 more with at least `emailSha1Lower`.
