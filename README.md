# Matplotlib wheels and release tooling

This repository contains what is needed to build release artifacts (wheels and
sdist) for the official [Matplotlib releases to
PyPI](https://pypi.org/project/matplotlib/) as well as nightly wheel builds which
are uploaded to
[anaconda.org/scientific-python-nightly-wheels/matplotlib](https://anaconda.org/scientific-python-nightly-wheels/matplotlib).

This was adapted for Matplotlib from [scipy/scipy-release](https://github.com/scipy/scipy-release).

This repository is minimal on purpose, for security reasons it contains only what is absolutely necessary. The repository settings are stricter than on the main [matplotlib/matplotlib](https://github.com/matplotlib/matplotlib/) repository, for example:

- only the release & CI team has write access
- for PRs from anyone without write access, CI will always need manual approval
- linear history is required
- GitHub actions are whitelisted, only the necessary ones will be allowed
- no caching allowed, only clean builds from scratch
- no self-hosted runners are allowed

See [numpy#29178](https://github.com/numpy/numpy/issues/29178) for more context.


## Branches and tags

The `main` branch of this repository is meant to stay in sync with the `main` branch
of the [matplotlib/matplotlib](https://github.com/matplotlib/matplotlib) repository. It runs scheduled builds
as cron jobs twice a week, and uploads nightlies to 
[https://anaconda.org/scientific-python-nightly-wheels/matplotlib](anaconda.org/scientific-python-nightly-wheels/matplotlib).

For Matplotlib releases, the branch naming should match those of the main
`matplotlib/matplotlib` repository, e.g., `v3.10.x` for the 3.10.x releases.

Which branch, commit or tag is built when a set of wheel builds is triggered is
controlled by the `SOURCE_REF_TO_BUILD` variable at the top of
`.github/workflows/wheels.yml`.


## Build reproducibility

Wheel builds being fully reproducible is a long-term goal for this repository.
All dependencies and actions must be pinned, which allows us to already be
close to full reproducibility. However, we don't (yet) have full control over
all ingredients that go into a wheel build, e.g. the containers which GitHub
Actions provide may change over time.


## Trusted publishing and attestations

The release builds in this repository should be using trusted publishing to
publish directly to PyPI (and TestPyPI), including attestations. Triggering
a release build has to be done by the `workflow_dispatch` in the
[Actions UI in this repository](https://github.com/matplotlib/matplotlib-release/actions/workflows/wheels.yml),
selecting `pypi` or `testpypi` as the target. This will use a GitHub Actions
"environment" of the same name - before the uploads to PyPI actually happen,
the release manager can go in and inspect the build logs and produced wheels.
Once those look good, the release manager can finalize the release from the
[deployments page in this repository](https://github.com/matplotlib/matplotlib-release/deployments).


## Software Bill of Materials

We aim to start producing SBOMs and ship them inside Matplotlib wheels uploaded to
PyPI, however as of today that is not implemented.


## Security

To report a security vulnerability for Matplotlib itself, please see
[the security policy on the main repo](https://github.com/matplotlib/matplotlib/?tab=security-ov-file#readme).

To discuss a supply chain security related topic for the code in this
repository, please open an issue on this repository if it can be discussed in
public, and otherwise please follow the security policy on the main repo.
