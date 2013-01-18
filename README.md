# openbadges-verifier

Verifying recipients of OpenBadges

# Prerequisite knowledge

A **badge assertion** looks something like this:

```json
{
  "recipient": "sha256$2ad891a61112bb953171416acc9cfe2484d59a45a3ed574a1ca93b47d07629fe",
  "salt": "hashbrowns",
  "evidence": "/badges/html5-basic/bimmy",
  "expires": "2013-06-01",
  "issued_on": "2011-06-01",
  "badge": {
    "version": "0.5.0",
    "name": "HTML5 Fundamental",
    "image": "/img/html5-basic.png",
    "description": "Knows the difference between a <section> and an <article>",
    "criteria": "/badges/html5-basic",
    "issuer": {
      "origin": "http://p2pu.org",
      "name": "P2PU",
      "org": "School of Webcraft",
      "contact": "admin@p2pu.org"
   }
  }
}
```

In order for a badge to be issued, the issuing organization has a URL on
their server that responds an assertion and the content-type
`application/json`, for example. This is what we call a **hosted assertion**.

In addition to the above, we have the concept of a **baked badge**. A
baked badge is a PNG that has, embedded in the `iTXt` metadata field of
the image, the URL pointing to the assertion that proves that a user has
earned the badge being represented (with the keyword `openbadges`)

By default, if the badge isn't already baked when going into the
backpack, we automatically do that and show the baked version of the
badge to the user.

# So what's this?

When someone sees a baked openbadge out in the wild, they should be able to:

1) Unpack the data that's in the badge; and
2) Verify that the user displaying the badge actually earned the badge.

For number one, this means having a tool that goes into the PNG, pulls
out the URL and gets the data at the endpoint. For number two, that
means asking the inquiring party who they think the badge belongs to,
hashing it and comparing to the calculated hash of the recipient.

# Proposed user experience

1) A user finds a baked badge out in the wild

2) She saves it/copies it, and uploads it (or drags & drops, or pastes)
into the verifier

3) Verifier screen shows all of the metadata in the badge (name of
badge, description, criteria, etc.) and presents a form field for
entering an email address to verify against the badge

4) On submit, verifier says "yes, this badge belongs to that user" or "no, that badge belongs to another user".

# Necessary libraries

* Something to read PNG chunks out of a PNG.
  * node: https://github.com/mozilla/openbadges-bakery (or, more low-level, https://github.com/brianloveswords/streampng)
  * ruby: https://github.com/wvanbergen/chunky_png
  * python: http://packages.python.org/pypng/png.html
