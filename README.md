# Unofficial Elm for Raspberry Pi

**Built on Raspberry Pi 4 Model B with Raspbian GNU/Linux 10 (32 bits).**

**Tested on:**
* Raspberry Pi 4 Raspbian GNU/Linux 10.

*Please report any success or failure on others ARM platforms.*

# Installation
## Last version
**elm** + **elm-format** (currently elm 0.19.1 and elm-format 0.8.2):
```
curl -L https://github.com/dmy/elm-raspberry-pi/raw/master/elm.tar.gz | sudo tar zxC /usr/local/bin
```

## Specific versions
See [Releases](https://github.com/dmy/elm-raspberry-pi/releases/).

# Known issues
* Because [SMP is disabled by mistake on ARMv7](https://gitlab.haskell.org/ghc/ghc/issues/13007), the compiler will use a single core.

# Support
As these binaries are unofficial, please always confirm an Elm bug on a platform officially supported before opening an issue at https://github.com/elm.

If you are not sure or if the bug is specific to ARM, report it here instead.

# Building from source
- Clone the official `elm` or `elm-format` repository
- Checkout the version tag
- Apply the patches included in this repository using `git am`
- `cabal new-update`
- `cabal new-configure --ghc-option=-split-sections`
- `cabal new-build`
