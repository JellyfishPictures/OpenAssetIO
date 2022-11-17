# The contribution process

This project operates a Pull Request model, using the [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow)
approach. The rough outline is as follows:

0. Ensure there is a suitable [CLA](#contribution-sign-off) in place for
   you, or your organization
1. Chat with the Trusted Committers and community to define the scope of
   work
2. Fork the repo, and work in a branch
3. Create a Pull Request for Code Review
4. Address any comments
5. Work is merged

The main aim of the process is to keep people in the loop - avoiding any
surprises later on. This helps avoid churn/frustration for all involved.

## Defining the scope of work

The first stage defines a shared understanding of what work is to be
done, this should always be done in conjunction with the project's
Trusted Committers. The steps are:

1. Ensure you can describe the nature of your request or change, along
   with real-world use cases any why it is desirable.
2. Create a thread in the project [discussions](https://github.com/OpenAssetIO/OpenAssetIO/discussions/categories/ideas-and-change-proposals)
   area that describes the topic, this is to ensure:
    - There isn't any overlapping work already planned or in progress.
    - The change makes sense as part of any broader vision for the
      project.
    - There is agreement on next steps, which may include research
      and/or design phases.
3. Along with the trusted committers, create an issue using the
   What/Why/Acceptance Criteria outline that covers the definition of
   done for the work
4. Review the planned work with the team, and agree a testing strategy
   and target branch.
5. Trusted Committers will be assigned to review the work through to
   delivery.

## Development

We have a few golden rules for commits to release branches:

- All code should follow the project coding standards.
- `main` and maintenance branches should always be functional, once we
  pass alpha and beta, they must always be release ready.
- Commits should form meaningful atomic units of change. The build
  should never be (intentionally) broken between commits merged to
  release branches.
- Fix bugs in the oldest branch and merge forwards, do not cherry-pick
  back.
- All Pull Requests must be reviewed by someone who did not write the
  code.

### Working process

The process for feature development is as follows (we use the [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow) process):

1. Create a fork of the repository, and a working branch and begin work.
     - Keep the appropriate Trusted Committers in the loop with
       regarding any in-flight issues.
     - Request in-progress reviews if necessary to validate design
       choices by creating a [WIP PR](PULL_REQUESTS.md#wip-prs) and
       @-ing a Trusted Committer.
     - Branches should be named using the
       `work/<issueNumber>-<camelCaseTitle>` convention.
2. Upon completion, prepare your Pull Request in accordance with the
   [Pull Request guidelines](PULL_REQUESTS.md), for larger work, this
   may be targeted at a feature branch, rather than a release branch.
3. Address any feedback with `fixup!` commits, creating a separate
   commit for each comment thread.
4. Once approved, rebase and squash any `fixups!` or `squash!` commits
   authored as separate commits to ease review and re-request a review.
5. The Pull Request will be merged *by a reviewer* into the target
   branch.

> IMPORTANT: You should never merge your own PR unless instructed to by
> the reviewer.

### Coding standards

The mix of languages used in this project means there are some important
guidelines on how to format code and documentation written for the
project. These are outlined in the [coding style](CODING_STYLE.md) and
[coding standards](CODING_STANDARDS.md) guides.

### Change Log

The Change Log should always be updated during development work, rather
than at release time. This ensures it is consistent with the code base
and that change notes are written at the time of greatest understanding.

Any commit that makes a user facing change should update the log. See
the [Recording user facing changes in the release notes](CHANGES.md)
section for more details.

### Contribution sign off

OpenAssetIO is licensed under the [Apache 2.0 license](../../LICENSE).
All contributions to the project must abide by that license.

Contributions to OpenAssetIO require the use of the [Developer’s Certificate of Origin 1.1 (DCO)](https://developercertificate.org).
All commits must be signed-off as follows, before merge, to indicate
that the submitter accepts the DCO:

```
Signed-off-by: Jóna Jónsdóttir <jona.jonsdottir@example.com>
```

This can be added automatically to a commit using `git commit -s`.

We also require commits to be [GPG signed](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits).

In addition, contributors are required to complete either the Individual
or Corporate Contribution License Agreement. Please contact one of the
trusted committers for more information.

### Release Process

OpenAssetIO releases are managed via git tags, and make use of [SemVer](https://semver.org).
All components within the same repository share a version, and will be
released together.

To make a new OpenAssetIO release, follow this procedure.

- Create a work branch in a forked repository.
- Update [`RELEASE_NOTES.md`](../../RELEASE_NOTES.md) version placeholder
  to be the concrete release number. (For example, change v1.0.0-alpha.x
  to v1.0.0-alpha.5).
- Update version numbers in :
  - [pyproject.toml](../../pyproject.toml) (Under `[project]` section).
  - [CMakeLists.txt](../../CMakeLists.txt) (An argument to `project()`,
      no need to update for an alpha or beta increment).
- If there is an ABI change (major release), update
  `set(core_abi_version X)` in
  [`src/openassetio-core/CMakeLists.txt`](../../src/openassetio-core/CMakeLists.txt)
  to the new major version - regardless of it's previous value (this may
  skip values).
- Commit changes using the title: `[Release] Bump version to <version>`.
- Have a final scan over the release notes to check for typos. If any
  are spotted, add a commit *before* the version bump to fix them.
- Create a PR into the primary repo, review and merge as normal.

> **Warning**
>
> Upon merging to main, several actions will kickoff against your merge
> commit. Notably among these is
> [Build wheels](https://github.com/OpenAssetIO/OpenAssetIO/actions/workflows/build-wheels.yml),
> which makes the python release artifacts available for upload, by the
> [Deploy PyPI](https://github.com/OpenAssetIO/OpenAssetIO/actions/workflows/deploy-pypi.yml)
> action.
>
> The [Build wheels](https://github.com/OpenAssetIO/OpenAssetIO/actions/workflows/build-wheels.yml)
> action must be successfully run before a release is created. Wait
> until this action is finished before continuing on.

- Now the codebase is all setup, create a [new Release](https://github.com/OpenAssetIO/OpenAssetIO/releases/new)
  in GitHub.
  - Set the tag to be the version number with a `v` prefix, eg
    `v1.0.0-alpha.5`, you should be creating a new tag.
  - Set the title to be the same as the tag, eg `v1.0.0-alpha.5`.
  - Copy the release notes for this release into the description:
    - Do not include the version title.
    - Remove line wrapping from the release notes, each note
      should becomes a single line.
      (vim users: `set tw=9999`, `gg<S-V><S-G>gq`)
  - If this is an alpha or beta release, ensure `Set as a pre-release`
    checkbox is checked.
  - Click "Publish Release"
- In response to a release being published, the
  [Deploy PyPI](https://github.com/OpenAssetIO/OpenAssetIO/actions/workflows/deploy-pypi.yml)
  action will be invoked. Monitor this to ensure PyPI wheels are
  uploaded correctly.
- You're done! Take a break and relax!

## Further reading

- [Coding style](CODING_STYLE.md)
- [Coding standards](CODING_STANDARDS.md)
- [Pull Requests](PULL_REQUESTS.md)
- [Commits and commit messages](COMMITS.md)
- [Code Reviews](CODE_REVIEWS.md)
- [Updating the release notes](CHANGES.md)


## Trusted Committers

### Foundry
- @foundrytom [tom@foundry.com](mailto:tom@foundry.com)
- @feltech