# Unofficial Elm for Raspberry Pi

## Installation
```
curl -L https://github.com/dmy/elm-raspberry-pi/releases/latest/download/elm.tar.gz | sudo tar zxC /usr/local/bin
```
The last release includes:
* [**elm**](https://github.com/elm/compiler) 0.19.1
* [**elm-format**](https://github.com/avh4/elm-format) 0.8.2
* [**elm-json**](https://github.com/zwilias/elm-json) 0.2.3

See [Releases](https://github.com/dmy/elm-raspberry-pi/releases/) to install specific older versions.

Built and tested on Raspberry Pi 4 Model B with Raspbian GNU/Linux 10 (32 bits).

*Please report any success or failure on others ARM platforms.*

## Known issues
* Because [SMP is disabled by mistake in ghc on ARMv7](https://gitlab.haskell.org/ghc/ghc/issues/13007), the compiler will use a single core.

## Support
As these binaries are unofficial, please always confirm a bug on a platform officially supported before opening an issue at the official repositories.

If you are not sure or if the bug is specific to ARM, report it [here](https://github.com/dmy/elm-raspberry-pi/issues) instead.

## Building from source
On Raspbian GNU/Linux 10:

### elm & elm-format
- `sudo apt-get install ghc cabal-install`
- Clone the official `elm` or `elm-format` repository
- Checkout the version tag
- Apply the patches included in this repository using `git am`
- `cabal new-update`
- `cabal new-configure --ghc-option=-split-sections`
- `cabal new-build`

### elm-json
- `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
- `git clone https://github.com/zwilias/elm-json.git`
- `cd elm-json`
- Checkout the version tag
- `cargo build --release`
