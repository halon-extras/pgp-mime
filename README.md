# PGP/MIME

PGP/MIME encrypt/decrypt a MIME message.

## Installation

Follow the [instructions](https://docs.halon.io/manual/comp_install.html#installation) in our manual to add our package repository and then run the below command.

### Ubuntu

```
apt-get install halon-extras-pgp-mime
```

### RHEL

```
yum install halon-extras-pgp-mime
```

## Exported functions

These functions needs to be [imported](https://docs.halon.io/hsl/structures.html#import) from the `extras://pgp-mime` module path.

### pgp_sign_mime(mail, privkeyrings [, options])

PGP sign a MIME message. Invalid or missing arguments will case an exception to be thrown.

**Params**

- mail `MailMessage` - The mail message to sign (**Required**)
- privkeyrings `array` - The private key/keyrings to use. If a keyring contains multiple keys they will all be used. (**Required**)

The following options are available in the **options** array.

- profile `string` - The profile to use (One of `default`, `rfc4880` or `rfc9580`)

**Returns**

An associative array with a `result` (boolean) whether the mail message was successfully signed.

### pgp_verify_mime(mail, pubkeyrings [, options])

PGP verify a MIME message. Invalid or missing arguments will case an exception to be thrown.

**Params**

- mail `MailMessage` - The mail message to decrypt (**Required**)
- pubkeyrings `array` - The private key/keyrings to use. If a keyring contains multiple keys they will all be used. (**Required**)

The following options are available in the **options** array.

- profile `string` - The profile to use (One of `default`, `rfc4880` or `rfc9580`)
- strip_signature `boolean` - If the signature should be removed (altering the message). The default is `true`.

**Returns**

An associative array with a `result` (boolean) whether the mail message was successfully verified.
If the `result` is `true` a `signers` (array) containing the signers public keys is set.

### pgp_encrypt_mime(mail, pubkeyrings [, privkeyrings [, options]])

PGP encrypt a MIME message. Invalid or missing arguments will case an exception to be thrown.

**Params**

- mail `MailMessage` - The mail message to encrypt (**Required**)
- pubkeyrings `array` - The public key/keyrings to use. If a keyring contains multiple keys they will all be used. (**Required**)
- privkeyrings `array` - The private key/keyrings to use (in case you also want to sign the MIME message). If a keyring contains multiple keys they will all be used.

The following options are available in the **options** array.

- profile `string` - The profile to use (One of `default`, `rfc4880` or `rfc9580`)

**Returns**

An associative array with a `result` (boolean) whether the mail message was successfully encrypted.

### pgp_decrypt_mime(mail, privkeyrings [, pubkeyrings [, options]])

PGP decrypt a MIME message. Invalid or missing arguments will case an exception to be thrown.

**Params**

- mail `MailMessage` - The mail message to decrypt (**Required**)
- privkeyrings `array` - The private key/keyrings to use. If a keyring contains multiple keys they will all be used. (**Required**)
- pubkeyrings `array` - The private key/keyrings to use (in case you also want to verify the MIME message). If a keyring contains multiple keys they will all be used.

The following options are available in the **options** array.

- profile `string` - The profile to use (One of `default`, `rfc4880` or `rfc9580`)

**Returns**

An associative array with a `result` (boolean) whether the mail message was successfully decrypted.
If the `result` is `true` and `pubkeyrings` was provided a `signers` (array) containing the signers public keys is set.

### pgp_is_signed_mime(mail)

Check if a MIME message is signed using PGP.

**Params**

- mail `MailMessage` - The mail message to check (**Required**)

**Returns**

A boolean `true` if the message is signed using PGP.

### pgp_is_encrypted_mime(mail)

Check if a MIME message is encrypted using PGP.

**Params**

- mail `MailMessage` - The mail message to check (**Required**)

**Returns**

A boolean `true` if the message is encrypted using PGP.

## Examples

```
import $pubkeyring from "txt!test.pub.asc";
import $privkeyring from "txt!test.priv.asc";
import { pgp_is_signed_mime, pgp_sign_mime, pgp_verify_mime, pgp_is_encrypted_mime, pgp_encrypt_mime, pgp_decrypt_mime } from "extras://pgp-mime";

pgp_sign_mime($mail, [$privkeyring], ["profile" => "default"]);
// pgp_verify_mime($mail, [$pubkeyring], ["profile" => "default"]);
// pgp_encrypt_mime($mail, [$pubkeyring], [$privkeyring], ["profile" => "default"]);
// pgp_decrypt_mime($mail, [$privkeyring], [$pubkeyring], ["profile" => "default"]);
// pgp_is_signed_mime($mail);
// pgp_is_encrypted_mime($mail);
```
