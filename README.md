# covidcotra

This is an experimental rust library for managing the data exchange for a
covid-19 contract tracing system.

## Concepts

Conceptionally there are three components:

- [`Identity`](https://docs.rs/covidcotra/latest/covidcotra/struct.Identity.html) represents a single device.
- [`Authority`](https://docs.rs/covidcotra/latest/covidcotra/struct.Authority.html) represents a trusted authority
  (for instance the ministry of health)
- [`ContactLog`](https://docs.rs/covidcotra/latest/covidcotra/struct.ContactLog.html) is a minimal abstraction over a
  contact log.

## Identity

The identity internally holds a unique ID (the
[`UniqueIdentity`](https://docs.rs/covidcotra/latest/covidcotra/struct.UniqueIdentity.html)).  This identity is only ever
transmitted to the central authority when the person behind this identity
tests positive.

An identity can be looked at in two other ways: a
[`ShareIdentity`](https://docs.rs/covidcotra/latest/covidcotra/struct.ShareIdentity.html) which is an identity which
is encrytped with the public key of the public authority and is only ever
shared with other devices.  The share ID should rotate regularly and can
be used by the central authorities to determine that a user saw another user.
Since the IDs rotate it's impossible (at least on this level) for a device
to determine that they have seen a device a second time.

Secondarily there is the [`HashedIdentity`](https://docs.rs/covidcotra/latest/covidcotra/struct.HashedIdentity.html).
This is a hashed version of the unique ID which can be used to "poll" for
updates or subscribe to a push channel.  The central authority cannot map
from hashed identity to unique identity but the other way round.  This means
that the unique identity (which eventually gets associated with a real human
identity if revealed) only gets known to the central authority under the
following circumstances:

- a user tests positive and reveals
- the central authority listens on device IDs walking around with a lot of
  devices deployed all over the place.
- a user submits a list of contacts they saw

When a user is revealed by testing positive they are encouraged to rotate
the ID.  Since other users unique IDs are also revealed through contact list
uploaded they are encouraged to rotate once they are either tested positive
themselves or tested negative.

This is a proof of concept [for this blog post about contact
tracing](https://lucumr.pocoo.org/2020/4/3/contact-tracing/).

License: Apache-2.0
