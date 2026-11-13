# fvdb-ok-to-test-sandbox

Public sandbox for testing the `/ok-to-test` CI approval flow before it goes
into fvdb-core and fvdb-reality-capture. It is not a project repository and
holds no code.

- `ok-to-test-target.yml` runs on `pull_request_target`. Runs from the repo's
  collaborators (which includes every openvdb org member) start straight away.
  Every other run waits at the `ci-external-ok-to-test` environment.
- `ok-to-test.yml` handles a comment `/ok-to-test <full commit SHA>` from an
  account with write access. It checks that the SHA is the PR's current head,
  then uses the `fvdb-ok-to-test` GitHub App to release the held runs for that
  commit: the gated runs above, and fork `pull_request` runs waiting for
  "Approve and run".
- `fork-pr-hold.yml` is a plain `pull_request` workflow, to observe GitHub's own
  fork-PR approval hold.

No job here has secrets or cloud credentials other than the App's private key,
which only `ok-to-test.yml` on `main` can read.

To try it, fork this repository, change any file and open a pull request.
