# Unofficial Elm for Raspberry Pi

## Installation
```
curl -L https://github.com/dmy/elm-raspberry-pi/releases/latest/download/elm.tar.gz | sudo tar zxC /usr/local/bin
```
The last release includes:
* [**elm**](https://github.com/elm/compiler) 0.19.1
* [**elm-format**](https://github.com/avh4/elm-format) 0.8.2
* [**elm-json**](https://github.com/zwilias/elm-json) 0.2.3
* [**elmi-to-json**](https://github.com/stoeffel/elmi-to-json) 1.3.0

See [Releases](https://github.com/dmy/elm-raspberry-pi/releases/) to install
specific older versions.

Built and tested on Raspberry Pi 4 Model B with Raspbian GNU/Linux 10 (32 bits).

*Please report any success or failure on others ARM platforms.*

## Node.js

To use Elm tools like [elm-test](https://www.npmjs.com/package/elm-test) or
[elm-doc-preview](https://www.npmjs.com/package/elm-doc-preview),
you will need a more recent version of Node.js.  
The following command will install Node.js v12:
```
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -
sudo apt-get install -y nodejs
```
Is is advised to configure npm to store packages in a user directory.  
Add to `~/.bashrc`:
```
NPM_PACKAGES="$HOME/.npm-packages"
PATH="$NPM_PACKAGES/bin:$PATH"
# Unset manpath so we can inherit from /etc/manpath via the `manpath` command
unset MANPATH  # delete if you already modified MANPATH elsewhere in your configuration
MANPATH="$NPM_PACKAGES/share/man:$(manpath)"
```
and run
```
npm config set prefix ~/.npm-packages
```
Then logout/login.

## elm-test
You can install `elm-test` globally:
```
npm install -g elm-test
```
But it won't run without an `elmi-to-json` binary suitable for the platform,
which is not available upstream, so you have to copy manually the one provided
by this release to the expected location:
```
cp /usr/local/bin/elmi-to-json $(npm config get prefix)/lib/node_modules/elm-test/node_modules/elmi-to-json/bin/elmi-to-json
```

## Known issues
* Because [SMP is disabled by mistake in ghc on ARMv7](https://gitlab.haskell.org/ghc/ghc/issues/13007),
the compiler will use a single core.

## Support
As these binaries are unofficial, please always confirm a bug on a platform
officially supported before opening an issue at the official repositories.

If you are not sure or if the bug is specific to ARM, report it
[here](https://github.com/dmy/elm-raspberry-pi/issues) instead.

## Building from source
On Raspbian GNU/Linux 10:

### elm / elm-format / elmi-to-json
- `sudo apt-get install ghc cabal-install`
- Clone the official repository
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

## Tests
All `elm/core` tests pass successfully:
```
elm-test 0.19.1
---------------

Running 1268 tests. To reproduce these results, run: elm-test --fuzz 100 --seed 132427106450815 /home/pi/dev/core/tests/tests/Main.elm


TEST RUN PASSED

Duration: 31305 ms
Passed:   1268
Failed:   0
```
