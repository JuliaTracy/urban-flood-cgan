# Maintainer release checklist

## Prepared locally

- Documentation-only Git repository contains no implementation source.
- Original `.pth` files are copied—not moved—to the ignored `release-assets/` directory.
- Source-free TorchScript generator exports are stored in the ignored `release-assets/` directory.
- `SHA256SUMS.txt` is generated from the exact files supplied to recipients.
- TorchScript outputs are numerically compared with the source generator before release.

## Before publishing

- Confirm the corresponding-author contacts and obtain approval for the access process.
- Ask the relevant institution to approve or replace `PRIVACY_NOTICE.md`, including the data controller, lawful basis, retention period, requester rights, and privacy contact.
- Choose and document the model license/access agreement; do not rely solely on a README restriction.
- Upload model files only to a storage system that can enforce registration if registration is mandatory.
- Never publish a reusable direct download URL in this repository.
- Test the approved download package on a clean machine.
- Record the package version, date, size, and SHA-256 values.

## GitHub history cleanup

- Make local backup refs or a `git bundle` before any remote rewrite.
- Replace the public default branch with an unrelated-history documentation commit.
- Remove or replace every remote branch and tag that references implementation commits, including the water-depth branch.
- Review pull requests, forks, Actions artifacts, releases, caches, and attached files for source exposure.
- Contact GitHub Support if cached commits or pull-request refs must be purged.
- Remember that previously cloned or forked copies cannot be recalled by rewriting the repository.
