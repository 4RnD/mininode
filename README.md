# mininode - Node.js for Embedded Systems
[![MIT Licensed](https://badges.frapsoft.com/os/mit/mit.svg?v=102)](https://github.com/mininode/mininode/blob/master/LICENSE.md)[![Build Status](https://travis-ci.org/mininode/mininode.svg)](https://travis-ci.org/mininode/mininode)[![Fuck it! Ship it!](https://img.shields.io/badge/FUCK%20IT!-SHIP%20IT!-brightgreen.svg)](https://hackernoon.com/fuck-it-ship-it-905afd092547#.tnzm8yqap)

This is [libuv][1], [http-parser][2], [mbedtls][3], [libslz][4], and [lowzip][5]
held together with [duktape][6]. The goal here is to produce an implementation
of the [Node.js 6.9.2 API][7] with a full-fledged Javascript interpreter 
in a single binary with no runtime dependencies other than a standard C library.

This aims to implement the Node.js standard library entirely in C.

This is very early in development. Any feedback is very welcome!

## NOTE: PRE-ALPHA QUALITY!

I've posted this for early feedback and to receive input during the development 
process, but this is not close to being usable. I intend to borrow a lot of 
code from [Dukluv][8] and track the upstream Duktape development process, but 
any thoughts (in, say, the form of issues) would be appreciated!

## Notes on Compatibility

While this aspires to be compatible with the Node 6.9.2 API, it does not aspire 
to be tit-for-tat compatible with every nuance of the V8 Node.js runtime. Since 
the standard library is written in C (as opposed to Javascript), it may not be 
possible to (for example) access the prototypes of built-in modules unless this 
has been explicitly implemented. Remote debugging in Chrome is challenging, but
I don't want to completely rule out the possibility.

So, while I'd like for the overall development experience to be very similar, 
code which is heavily dependent on the features and internal behavior of the 
mainline Node.js runtime will likely need to be ported.

## Build Instructions

This uses GNU make and it is currently being developed on MacOS X and Linux,
other systems are not yet supported but this should also work on Illumos/Solaris
and *BSD (possibly with minimal porting effort).

Either `gcc` or `clang` should work, just set the `CC` environment variable.

From a MacOS X system:

    git clone https://github.com/mininode/mininode.git
    cd mininode
    git submodule init
    git submodule update
    make defconfig
    make
    make install
    make uninstall

For use on embedded systems, musl libc and Linux are recommended (in general).

## Developing with Vagrant

The included `Vagrantfile` runs the Alpine Linux edge release and is meant for 
Virtualbox 5 on MacOS hosts. It mounts the mininode sources at `/mininode` via 
NFS. It will update to the latest edge release on the first boot.

Note that this assumes you've done `git submodule init` and 
`git submodule update` on the host system.

    vagrant up
    vagrant ssh
    cd /mininode
    make defconfig
    make
    make install

## What's the point?

This is intended aid in rapid development of small embedded clients, with 
realistic requirements of ~75mhz and 2MB RAM although less might be workable. 
Since this will likely never approach the performance of V8 as a javascript 
interpreter, you will almost certainly want to continue using mainstream 
Node.js for servers unless you are prepared to write the bulk of your hot path 
in C using duktape.

[1]: https://github.com/libuv/libuv.git
[2]: https://github.com/nodejs/http-parser.git
[3]: https://github.com/ARMmbed/mbedtls/tree/mbedtls-2.3
[4]: https://github.com/haproxy/libslz
[5]: https://github.com/svaarala/lowzip
[6]: http://duktape.org
[7]: https://nodejs.org/dist/v6.9.2/docs/api/
[8]: https://github.com/creationix/dukluv/tree/master/src

# Signed Tags

After mininode 1.0, tags will be signed using a key corresponding to this one:

    -----BEGIN PGP PUBLIC KEY BLOCK-----
    
    mQENBFjpWSYBCADU8KvSfJiVr03h7T07TFJFN2ZK7AAnqVFLqjePjVIy4SiHXI2k
    HVKPE6tQky6bsW6R+Cu1PxsssDEY/Ed/EvkXovEYxL0KM4ChKt5qDA/smGqtANwN
    BHrzIdgddzFO1HjvgUB/RYej38/41vaDM3n8Gn4ZGuERheGe6lGfMuPtxgTt79VW
    kaZgUzuBaQDqBSe6/oYvsJ0z+pBqj5+NC2jplwsfnn19MJqd4rBYBMYIeitxCGmk
    Rrs8pAH6hI33+sQEd9DhmUWCh2xkG0sAJaE9MbBIkkNFXOJJ/kb2F/jsvo4K/FTi
    Bjw9nnki6i6QHDn8N0ef0am1myS7QW2MUmw1ABEBAAG0KkFsZXggQ2F1ZGlsbCA8
    YWxleC5jYXVkaWxsQHByb3Rvbm1haWwuY29tPokBVAQTAQgAPhYhBCk2zBOl3lP0
    8S3Aw+OWGErEWH9dBQJY6VkmAhsDBQkDwmcABQsJCAcCBhUICQoLAgQWAgMBAh4B
    AheAAAoJEOOWGErEWH9dFRYH/0pOtG6A44vEL1SuJxN+4c8RYB6z3YQN10W/V0Ia
    YEDAJP8q5Hm7v/++UzeAHV+Yly8izgy0DGmQAqpHWA2OQo+IKu2Z9/8k1+CzLW1g
    yj+kHyrOcm6gqWqLrgHJfu7ONCsIyhcF8hNwlvTnsYQxC4/gfOufoedFhz/y0lZn
    z1Zw6tq4AfHmMa+jThGP3XaK9NxYVTGEeOxkJMv4Up8Nnq/3nGHdZnS5DL4vLGAj
    gAjtEn7Kfjoe6qkKMw2KSyw9EkV/Avncq0pu9cB6rOgx4fUDGpjQVnc5ZVbOeg1t
    tSiFV1qTfMjucpBt+Wc6q3qV9MLaN6pmO5/MjNxBl8RHKKy5AQ0EWOlZJgEIALSb
    4GY2WeTKemFSsGq9fLAuoFL+0hhHXMQ5abckw+vqSUg1cfmm0J374VGQVGXRjtaX
    Fkbaeb9930too7ZcO7HtxlwnpBmYlV4WZk3ns9CtlFpseG5vI9X9Dtz0HKc6wzjs
    AGyAxdcewGIhViWJAvu/kn8kLBWbv6iW5GOgFjQlKgsBcbQd2DYRhz0+hYQXwoDR
    W73F8IPRqMxAVc9aMJqJ5S99hT+RFNySrIFgw3kbxkIVTNBeywBFy3s0/6ep/N3h
    pCZ0iyGImtj4TWqt9XXYKKX5dah8HT9D8/ZfOB/H5JGCwAFEOvhB5iXwC7yt0ubz
    pkBv9rVac60d2CVisDsAEQEAAYkBPAQYAQgAJhYhBCk2zBOl3lP08S3Aw+OWGErE
    WH9dBQJY6VkmAhsMBQkDwmcAAAoJEOOWGErEWH9dRlAIAL0SgCgkg0oybHHYmMkx
    BcrSBKJ3i4gmgk40Dn16eNuN7SinigQpW22CVOwSIqT42c4a4XafsEyhY/nRiKrl
    p1c5wMEaDSiCJj7UqW01hDuhzs/pAPZsDunfbjfqMiTkBO9AWq+QnBuqSSBRy8Qr
    mxB5qZAqTZZbRijFYgKsBo//kSV0m/yqUzV/OGUsVY0EktQJyZP2+HAN7R+OjAcb
    1SjYi8UlvkUA+btMS7fMFSDmtxo9c7c60YACNeO4ZBqIDNBBA/kUCSpOhLBkriZx
    q+shygUZGmyocV9+n8Rt7mTdE5KjK8asJEHqJy1BRukmI5BxFBIkgbcaUkZAU+1Q
    d+E=
    =jgJd
    -----END PGP PUBLIC KEY BLOCK-----

