# MLTuno

**English** | [한국어](README_KO.md)

**Machine Learning–Based Autotuning**

MLTuno is a research framework under development for machine-learning-based
autotuning of applications and computing systems. It builds on
[ytopt](https://github.com/ytopt-team/ytopt), retaining ytopt's tuning interface
as the foundation for new learning and search methods.

## Approach

- **ytopt foundation:** use ytopt's problem definitions, search methods, and
  evaluation workflow as the common tuning interface.
- **Transfer-learning module:** incorporate
  [GC_TLA](https://github.com/tlranda/GC_TLA), a Gaussian-copula-based
  transfer-learning autotuning method (ICS 2023), as one module
  within MLTuno.
- **Ongoing research:** extend this work toward deep-learning-based transfer
  learning for HPC applications, within a broader ML-based autotuning framework.

## Status

Initial development. The repository starts from ytopt and includes GC_TLA as
an upstream submodule. Integration of GC_TLA and new transfer-learning methods
with the common ytopt interface is planned.

## Getting started

See the [repository setup guide](docs/GITHUB_SETUP_KO.md).
For baseline installation and tuning, follow the
[ytopt documentation](https://github.com/ytopt-team/ytopt#installation-instructions).
The initial Python package and tuning commands retain the `ytopt` name.

## References

- [ytopt](https://github.com/ytopt-team/ytopt)
- Randall et al. (2023).
  [Transfer-Learning-Based Autotuning Using Gaussian Copula](https://doi.org/10.1145/3577193.3593712).
  ICS 2023. [Code](https://github.com/tlranda/GC_TLA).

## License

The upstream BSD-2-Clause licenses and copyright notices are retained:
[ytopt](LICENSE.md) and
[GC_TLA](https://github.com/tlranda/GC_TLA/blob/master/LICENSE.md).
