# pyrrhula-builds

Images for [Pyrrhula](https://github.com/tuturu742/pyrrhula) organizations, built by
GitHub Actions.

Pyrrhula never builds images on its own machines. When someone presses **Build** on an
image, Pyrrhula dispatches the workflow in this repository with three inputs: the build id,
the exact image reference to push, and the Dockerfile, base64-encoded. The workflow:

- reads every input through `env:`, never by pasting it into a shell line;
- refuses a target outside the `PYRRHULA_TARGET_PREFIX` repository variable;
- builds from an empty context, so the image holds tools only and never a repository's files;
- pushes with this workflow's own `GITHUB_TOKEN`, so Pyrrhula never holds a push credential.

Pyrrhula then asks the registry for the digest itself, smoke-tests the image on the
organization's execution engine, and only then offers it as a build runtime, pinned by
digest.

The workflow is the reference one from Pyrrhula's `docs/builders/github-actions.yml`.
