---
description: This page describes about the available methods of installation.
icon: compact-disc
---

# Installing the Kernel

The kernel simulator can be installed on all the supported platforms. The installation steps are straightforward, but must be followed in order to ensure that the kernel starts right.

{% hint style="danger" %}
We don't support starting Nitrocid KS from another environment other than direct execution. If Nitrocid detects that you're running it in such a configuration, functionality will be limited for your security.
{% endhint %}

{% hint style="danger" %}
We no longer support 32-bit platforms; hence we only support ARM64 and AMD64. Find out why [here](https://officialaptivi.wordpress.com/2024/08/03/final-word-regarding-32-bit-support/).
{% endhint %}

Depending on your platform, the amount of disk space taken by KS and its runtime dependencies might vary. Select your platform below and follow the steps.

{% content-ref url="windows.md" %}
[windows.md](windows.md)
{% endcontent-ref %}

{% content-ref url="macos.md" %}
[macos.md](macos.md)
{% endcontent-ref %}

{% content-ref url="linux.md" %}
[linux.md](linux.md)
{% endcontent-ref %}

{% content-ref url="android.md" %}
[android.md](android.md)
{% endcontent-ref %}

{% hint style="info" %}
If you've installed a development version of Nitrocid, you may see a warning about such versions. You can, however, hide this warning by opting out of these messages by:

1. Executing the `settings` command
2. Opening the `General` section
3. Enabling the `Development notice acknowledged` option
{% endhint %}

## Verifying intregrity

You can verify the integrity of all the Nitrocid release assets using the two methods:

* GitHub Attestations
* Manual Hashing

### Attestations

GitHub have recently introduced a new feature that allows you to verify a binary artifact that a workflow has generated, called the [Attestations](https://github.blog/2024-05-02-introducing-artifact-attestations-now-in-public-beta/). To verify your download, once you've downloaded one of the Nitrocid zip files, follow these steps:

1. Install [GH CLI 2.49.0](https://github.com/cli/cli/releases/tag/v2.49.0) or higher.
2. Sign in to your GitHub account using `gh auth login`.
3. Run this command: `gh attestation verify <version>-bin.zip --owner Aptivi`, where `<version>` is a version of Nitrocid that you've downloaded.

If everything is OK, you should see the below message, such as one for 0.1.0.10:

```
Loaded digest sha256:6030eb1eb660f336d8b070202c598e8f51e50c8b9ca9084f30aa8d40ecbb996f for file://0.1.0.10-bin-lite.zip
Loaded 1 attestation from GitHub API
✓ Verification succeeded!

sha256:6030eb1eb660f336d8b070202c598e8f51e50c8b9ca9084f30aa8d40ecbb996f was attested by:
REPO               PREDICATE_TYPE                  WORKFLOW
Aptivi/NitrocidKS  https://slsa.dev/provenance/v1  .github/workflows/prepdraft.yml@refs/tags/v0.1.0.10
```

{% hint style="warning" %}
If you've seen this error message:

```
Loaded digest sha256:78fc7b18c2e5e2753934652d294456d11d8dadad6f638dedc31513c4570587a1 for file://0.1.0.10-bin-lite.zip
✗ Loading attestations from GitHub API failed

Error: failed to fetch attestations from Aptivi: HTTP 404: Not Found (https://api.github.com/orgs/Aptivi/attestations/sha256:78fc7b18c2e5e2753934652d294456d11d8dadad6f638dedc31513c4570587a1?per_page=30)
```

Then, your download is corrupt.
{% endhint %}

### Manual hashing

After you've downloaded the ZIP file, follow these steps:

1. Download the `hashsums.txt` file that matches your version.
2. Open the file to get expected hash sums.
3. Select one of the bin/lite versions and use the `sha256sum` command against the file.
4. Compare the output.

{% hint style="warning" %}
You can't check the integrity of non-binary ZIP files this way, because `hashsums.txt` doesn't contain info about them.
{% endhint %}
