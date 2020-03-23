title: Playing with WebTransport
date: 2020-03-23 22:50:10
tags: WebTransport
---

This post describes how to try [WebTransport](https://wicg.github.io/web-transport/) (QuicTransport) in Chrome Canary with a sample server provided in Chromium.

## Build QUIC transport simple server.

QUIC transport simple server is provided in Chromium as a sample server for `QuicTransport`. After getting Chromium's code, you can build its binary by

```
ninja -C out/debug quic_transport_simple_server
```

## Prepare certificates

You need a valid certificate and a pkcs8 format key to run the server. Handshake fails if your certificate is invalid or not trusted.

The script in `net/tools/quic/certs` could help you to generate a certificate/key pair for you, but you may need to correctly configure CN, SAN to match your hostname. Root CA should be trusted by Chrome.

## Run the server

Run server with certificate and key.

```
out/debug/quic_transport_simple_server --certificate_file=<certificate.pem> --key_file=<key.pkcs8> --port=<port>
```

## Run Chrome

Run Chrome with a couple of flags

```
Chrome --enable-experimental-web-platform-features --enable-quic --quic-version=h3-25 --origin-to-force-quic-on=<hostname:port>
```

My observation is [QuicTransport client](https://chromium.googlesource.com/chromium/src/net/+/8efd409806c64c06fa11b1d6b8a16c6590d4727b/quic/quic_transport_client.cc#137) only supports TLS 1.3 as handshake protocol and draft-ietf-quic-transport-25 as transport protocol, so `--quic-version=h3-25` is required. `--origin-to-force-quic-on` flag [allows unknown root](https://chromium.googlesource.com/chromium/src/net/+/8efd409806c64c06fa11b1d6b8a16c6590d4727b/quic/crypto/proof_verifier_chromium.cc#395) for your origin. WebTransport is still an experimental feature, so you need `--enable-experimental-web-platform-features` to enable it.


## References

- WebTransport: https://wicg.github.io/web-transport/
- The WebTransport Protocol Framework: https://tools.ietf.org/html/draft-vvv-webtransport-overview
- WebTransport over QUIC: https://tools.ietf.org/html/draft-vvv-webtransport-quic
- QuicTransport tracking bug: https://bugs.chromium.org/p/chromium/issues/detail?id=1011392
- Playing with QUIC: https://www.chromium.org/quic/playing-with-quic