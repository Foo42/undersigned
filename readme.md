# Undersigned

The purpose of this project is to unlock information when sufficient parties agree to some statement. This is enabled using Shamir's secret sharing scheme.

## Workflow

### Encoding
When a secret is encoded n keys are generated, of which m are are required to decode the secret. These can then be distributed to the parties.

### Decoding
The parties arrange to enter all their keys.

## Implementation.
### Initiation
1. A public private key pair is generated for each party using RSA.
2. The public key of each party is recorded against an identifier for the owning party. This may be published.
3. The private keys are distributed to the parties. A statement should be distributed with the keys explaining the circumstances they should be used, eg. the death of the secret master.
4. An initial version of the primary payload is created and encrypted using AES symmetric encryption.
5. The payload is published
6. They payload encryption key is encoded using SSSS
7. The generated SSSS key fragments are encrypted using the parties' public keys
8. The encrypted key fragments are published

## Design Considerations

### Design Goals
* Make the system as simple to use, especially during decryption
* Enable the decryption of the secret without additional private information from the secret's master.
* Make the system robust enough that the chance of being unable to decrypt the secret even with the keys is minimised


### What to Encode
Ideally we want to minimise the number of times keys have to be redistributed to avoid confusion, as such it makes sense not to encode details which may be subject to change. The simplest thing to do is encode a password or encryption key which can then be used to decrypt the main payload. This payload can then contain information which can be changed by the original author at will using the master decryption key which is possessed by them. Since this payload is critical, but encrypted, it probably makes sense to publish it somewhere.

### What to distribute
If we distribute the raw key fragments, then we cannot change the total number of keys (n), or the minimum required keys (m) without regenerating and then redistributing all the keys. This increases the chances for confusion should these parameters need to be changed at some point. An alternative would be to generate the key fragments, then encrypt them using regular encryption, and distribute to each party the encryption key to decrypt their actual Shamir key fragment. While this enables changing the SSSS parameters without key redistribution it introduces more steps in the process and more state which is required for successful decoding of the secret, since the encrypted SSSS key fragments must be maintained by the secret master. In addition, the secret master must be able to modify these encrypted SSSS fragments in order to change the SSSS parameters, which necessitates either privately maintaining copies of the distributed encryption keys if symmetric encryption is employed, or maintaining access to public keys in the case of asymmetric encryption.
