## SSH authentication
Symmetric keys are sued by SSH in order to encrypt the entie connection. The public/priavte asymmetric key pairs are used only by authentication, not encrypting the connection.
Two stages: 1. agree upon and establish encryption to protect future communication, using Diffe-Hellman algorithm to generate a session encryption/decryption key, and this key will be used to encrypt the entire connection. 2. use private/public key pair to authenticate the client to the server.
Diff-Hellman algorithm: similarly as color mixing.
