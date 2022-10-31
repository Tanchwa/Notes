- TLS uses the public key infrastructure, or PKI. This consists of three players: the server, the client, and a third party authenticating server.
  id:: 62ed743a-264e-4860-9b54-ca747cdd5020
- the server has its own key pair, the private key for decrypting and the public key, that is stored in a certificate (or certificate signing request)
- the server's certificate is then signed by a third party authority's private key
- the client that needs the public key of the server to encrypt their data knows its safe because they have the public key of the third party [[certificate authority]] to unlock/ verify the authenticity of the server public key
- client certificates can also be used to verify the client as well, and this can also be signed by a [[certificate authority]], but is usually done in house rather than a third party
  id:: 62ed74ec-cd5d-4807-a58c-ccc63445d653
  think, this is for authenticating users to a resource, why does the public authority care?