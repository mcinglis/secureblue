
**secureblue** is a set of [bootable container images], based atop the
[Fedora Atomic Desktops], built with [BlueBuild].

This repository is a soft-fork of the [upstream] secureblue repository, enabling
easy personal customization of the system.

[bootable container images]: https://docs.fedoraproject.org/en-US/bootc/getting-started/
[Fedora Atomic Desktops]: https://pagure.io/workstation-ostree-config
[BlueBuild]: https://blue-build.org/
[upstream]: https://github.com/secureblue/secureblue


## Fork Setup

To get a new GitHub fork of secureblue building successfully on GitHub Actions,
you need to setup two secrets on the repository's settings, via **Settings >
Secrets and variables > Actions**.

### `SIGNING_SECRET`

This is the cosign private key. To generate a new key pair, per
[BlueBuild's documentation](https://blue-build.org/how-to/cosign/), run:

    skopeo generate-sigstore-key --output-prefix cosign 

Then:

- copy the contents of `cosign.private` into a new `SIGNING_SECRET` secret,
- remove that file, and
- commit the updated `cosign.pub` file.

### `KERNEL_PRIVKEY`

This is the SecureBoot signing key. To generate a new key pair, run:

    openssl req -config ./files/scripts/certs/openssl.cnf \
        -new -x509 \
        -newkey rsa:2048 \
        -nodes -days 36500 \
        -outform DER \
        -keyout './private_key.priv' \
        -out './files/system/etc/pki/akmods/certs/akmods-secureblue.der'

Then:

- copy the contents of `private_key.priv` into a new `KERNEL_PRIVKEY` secret,
- delete the `private_key.priv` file, and
- commit the updated `akmods-secureblue.der` file.

