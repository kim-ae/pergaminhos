## Pre-requisits
* [NerdFonts](https://www.nerdfonts.com/#features): [Here is a script](https://github.com/kim-ae/environment-setup/blob/main/scripts/bootstrap.sh#L12-L17) on how to install on linux mint. It is also possible to install using [oh-my-posh command](https://ohmyposh.dev/docs/installation/fonts)
* You need to be using one of those shells: bash, cmd, elvish, fish, nu powershell, xonsh and zsh.
* You need to have curl installed.
* Your terminal must at least support ANSI 256 color code, for that you can get the [this script] (https://github.com/kim-ae/environment-setup/blob/main/tools/.functions.zsh#L21-L28)and execute, it should show 256 colors like this:
  ![[ansi-color-code.png]]
## How to install
1. Install oh my posh: `curl -s https://ohmyposh.dev/install.sh | bash -s`
2. Enable it in your rc file by adding the following in the end of the file: `eval "$(oh-my-posh init zsh --config sometheme)"`, the theme names can be found [here](https://ohmyposh.dev/docs/themes)
3. Add below a function called `set_poshcontext` to enable dynamic env variables to run scripts to add more information in your terminal segments, see an example [here](https://github.com/kim-ae/environment-setup/blob/main/configs/set_poshcontext.sh)

## I want to customize
1. You can extract configuration from some pre built theme:  `oh-my-posh config export --config easy-term --format toml --output kim7s.easy-term.toml`, it accepts formats: toml, json and yaml
2. After that you just need to pass in the init configuration your file: `eval "$(oh-my-posh init zsh --config <path-to-file>/kim7s.easy-term.toml)"`;
3. To adjust your theme easily you can disable oh my posh cache by using the command `oh-my-posh enable reload`

## I want some cool ready to use theme
You can use my theme that I have publicly available on my [environment setup](https://github.com/kim-ae/environment-setup/blob/main/configs/kim7s.darkblood.toml) repository, it looks like this:
![[kim7s-ohmyposh-theme.png]]
My theme configurations:
### Upper left segment
* Current path
* git context whenever the folder has .git folder
## Upper right segment
* Tailscale vpn ip when tailscale is enabled
* Tailscale status
* nodejs version if the folder has package.json
* nodejs project version and name if folder has package.json
* It will show execution time if it took more than 100ms
### Lower left segment
* Simple indicating if the last command was successful or failed
* Current time
### Lower right segment (Dynamic segment)
* Shows kubectl current context when k, kubectl command is written;
* shows AZ subscription when az command is written;
Keep in mind that for that to show you must write the command and add a space, also the right segment will not disappear unless it is overwritten by another dynamic right segment

### Transient prompt
* Current time
### Customization
If you do not want to use tailscale segment you do not need to use the set_poshcontext, please remove the segments inside the .toml file as well.