## Verifying FastBCP Linux Binary Integrity and Authenticity

The zip file contains 3 files:
- `FastBCP`
- `FastBCP.sha256sum.txt`
- `FastBCP.sha256sum.txt.asc`

To check the integrity of your `FastBCP` file, generate its SHA256 sum and compare it with the sum present in `FastBCP.sha256sum.txt`:

```bash
sha256sum -b FastBCP
```

	2a44f461eb444506fc9de1f1ad09f6ccf441c76b07dd6ac6e4e46db2d61d9f3e *FastBCP

```bash
cat FastBCP.sha256sum.txt
```

	2A44F461EB444506FC9DE1F1AD09F6CCF441C76B07DD6AC6E4E46DB2D61D9F3E  FastBCP%

Or you can verify its integrity using a single command:
```bash
sha256sum -c FastBCP.sha256sum.txt
```

	FastBCP: OK

### Authenticity check

To verify the authenticity of `FastBCP.sha256sum.txt`, check the signature of `FastBCP.sha256sum.txt.asc` by following the steps below.

#### Download the FastBCP signing key:

```bash
gpg --keyserver keys.openpgp.org --recv-keys 37007D091BF7DCC74D208A1F869511C7130465C8
```

	gpg: key 869511C7130465C8: "Francois Pacull <francois.pacull@architecture-performance.fr>" not changed
	gpg: Total number processed: 1
	gpg:              unchanged: 1

You can check that the key was properly imported:

```bash
gpg --list-key --with-fingerprint 130465C8
```

The output should contain 3700 7D09 1BF7 DCC7 4D20  8A1F 8695 11C7 1304 65C8 and look like this:

	pub   rsa4096 2025-04-11 [SC]
	      3700 7D09 1BF7 DCC7 4D20  8A1F 8695 11C7 1304 65C8
	uid           [ultimate] Francois Pacull <francois.pacull@architecture-performance.fr>
	sub   rsa4096 2025-04-11 [E]


#### Verify the authenticity of `FastBCP.sha256sum.txt`

```bash
gpg --verify FastBCP.sha256sum.txt.asc FastBCP.sha256sum.txt
```

	gpg: Signature made Fri 11 Apr 2025 06:18:17 PM CEST
	gpg:                using RSA key 37007D091BF7DCC74D208A1F869511C7130465C8
	gpg: Good signature from "Francois Pacull <francois.pacull@architecture-performance.fr>" [ultimate]


The output of the last command should tell you that the file signature is `good` and that it was signed with the 37007D091BF7DCC74D208A1F869511C7130465C8 key.
