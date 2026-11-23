# Contributing

Thank you for helping improve this project.

1. Open an issue describing the problem or idea before larger changes.
2. Fork the repository, make a branch, and check that `pixi run reproduce`
   still works on your computer.
3. Open a pull request. The Reproduce check must pass before it is merged.

Please also:

- Keep raw data out of Git: `scripts/download_data.py` downloads it.
- Commit `pixi.toml` and `pixi.lock` together.
- Record analysis decisions, and why they were made, in `docs/decisions.md`.

Everyone taking part follows the [code of conduct](CODE_OF_CONDUCT.md).
