# Unofficial Elm for Raspberry Pi

## Install
```
curl -L https://github.com/dmy/elm-raspberry-pi/releases/latest/download/elm.tar.gz | sudo tar zxC /usr/local/bin
```
The last release includes:
* [**elm**](https://github.com/elm/compiler) 0.19.1
* [**elm-format**](https://github.com/avh4/elm-format) 0.8.2
* [**elm-json**](https://github.com/zwilias/elm-json) 0.2.7 (newer official Arm releases might be available [here](https://github.com/zwilias/elm-json/releases))
* [**elmi-to-json**](https://github.com/stoeffel/elmi-to-json) 1.3.0 (needed by `elm-test`)

See [Releases](https://github.com/dmy/elm-raspberry-pi/releases/) to install
specific older versions.

## Compatibility 
* Built and tested on Raspberry Pi 4 Model B with Raspbian GNU/Linux 10 Buster (32 bits).
* **It should work on all Raspberry Pis with Raspbian 10 Buster**, and more generally all Debian 10 based Arm systems.
* It will **not work** on Raspbian/Debian 8 Jesse and 9 Stretch because some linked libraries are not compatible.
* It has also been reported to work on:
    - Raspberry Pi 4 with Ubuntu 19.10 (armhf+raspi3)

If you have an aarch64 system with a 64 bits operating system, you will need 32 bits compatibility libraries and kernel option.

*Please report any success or failure on others Arm platforms.*

## Uninstall
```
$ sudo rm /usr/local/bin/elm{,-format,-json,i-to-json}
```

## Node.js

To use Elm tools like [elm-test](https://www.npmjs.com/package/elm-test) or
[elm-doc-preview](https://www.npmjs.com/package/elm-doc-preview),
you will need Node.js.  
The following command will install Node.js v10 and `npm`:
```
sudo apt-get update
sudo apt-get install nodejs npm
```

It is advised to configure npm to store packages in a user directory.  
Add to `~/.bashrc`:
```
NPM_PACKAGES="$HOME/.npm-packages"
export PATH="$NPM_PACKAGES/bin:$PATH"
# Unset manpath so we can inherit from /etc/manpath via the `manpath` command
unset MANPATH  # delete if you already modified MANPATH elsewhere in your configuration
export MANPATH="$NPM_PACKAGES/share/man:$(manpath)"
unset NPM_PACKAGES
```
and run
```
npm config set prefix ~/.npm-packages
```
Then logout/login.

You probably want to update to the latest npm version at this point:
```
npm install -g npm@latest
```
Then logout/login again.

## elm-test
You can install `elm-test` globally:
```
npm install -g elm-test --ignore-scripts
```
But it won't run without an `elmi-to-json` binary suitable for the platform,
which is not available upstream.
So we will make a link, in the expected location, to the one provided by this release:
```
ln -sft $(npm config get prefix)/lib/node_modules/elm-test/node_modules/elmi-to-json/bin /usr/local/bin/elmi-to-json
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
On Raspbian GNU/Linux 10 Buster:

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
