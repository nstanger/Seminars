= Seminar script

== Initial setup

The Tart images aren’t large enough (45GB), use UTM instead (default 64GB, 15GB remaining after install).

* Add shared folder(s) to make it easier to install things like Xcode and also to copy commands from this script.
* Create an administrator `nstanger` user.
* Change machine name to “Nigel’s-Virtual-Machine”.
* Grant Terminal access to modify apps (Privacy & Security > App Management). Make sure it is actually turned **ON**.
* Install Xcode and command line tools:
  * Download Xcode 26.3 from https://download.developer.apple.com/Developer_Tools/Xcode_26.3/Xcode_26.3_Apple_silicon.xip (requires Apple ID sign-in)
  * `xcode-select --install`
  * `sudo xcodebuild -license accept`

== Install Nix

**Clone the VM image before continuing.** Delete and re-clone to start from a clean slate.

* `curl -sSf -L https://install.lix.systems/lix | sh -s -- install` (1:15)
* Open new Terminal window to update path (or run `. /nix/var/nix/profiles/default/etc/profile.d/nix-daemon.sh`).
* `cd Documents`
* `nix shell nixpkgs#git` (between 1:30 and 3:00?)
* `git clone https://github.com/nstanger/nix.git`
* `cd nix`
* Change stateVersion to 5 in `darwin/default.nix`.
* Prevent griping about “”Unexpected files in /etc”:
    ```sh
    sudo mv /etc/nix/nix.conf /etc/nix/nix.conf.before-nix-darwin
    sudo mv /etc/nix/nix.custom.conf /etc/nix/nix.custom.conf.before-nix-darwin
    sudo mv /etc/shells /etc/shells.before-nix-darwin
    sudo mv /etc/zshenv /etc/zshenv.before-nix-darwin
    ```
* `nix build --extra-experimental-features "nix-command flakes" --extra-deprecated-features broken-string-escape ./#darwinConfigurations.Nigels-Virtual-Machine.system` (this will take quite a while)
* `sudo ./result/sw/bin/darwin-rebuild switch --flake ~/Documents/nix/.#` (3:30)
* Drop the following which cause issues:
  * LaunchBar (404: homebrew/default.nix)
  * TEXMF environment variables (no TeXlive: home-manager/default.nix)
* Open new Terminal window.
