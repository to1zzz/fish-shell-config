# My fish config

1. reef
cargo install reef-shell
git clone https://github.com/ZStud/reef.git /tmp/reef
fish /tmp/reef/fish/install.fish
rm -rf /tmp/reef
reef persist state
reef confirm on

2. PatrickF1/fzf.fish
fisher install PatrickF1/fzf.fish

3. zoxide
cargo install zoxide

4. atuin
cargo install --locked atuin

5. reef-tools
git clone https://github.com/ZStud/reef.git
cd reef
fish fish/install.fish --tools
+ripgrep, fd, bat
