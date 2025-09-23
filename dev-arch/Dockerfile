FROM archlinux:latest

ARG ENABLE_C=false
ARG ENABLE_NODE=false
ARG ENABLE_PYTHON=false
ARG ENABLE_RUST=false

RUN pacman -Syu --noconfirm
RUN pacman -S --noconfirm \
  base-devel \
  git \
  git-cliff \
  github-cli \
  ghq \
  tig \
  zsh \
  neovim \
  tmux \
  wget \
  curl \
  unzip \
  openssh \
  sudo \
  mise \
  python \
  less \
  gcc \
  clang \
  bat \
  fd \
  ripgrep \
  lsd \
  tokei \
  zoxide \
  fzf \
  trash-cli

RUN groupadd -r developer && useradd -m -r -g developer -s /usr/bin/zsh developer && mkdir /home/developer/repos/
RUN echo "developer ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
WORKDIR /home/developer/repos/dotfiles-public
USER developer

ADD https://github.com/yuma140902/dotfiles-public.git /home/developer/repos/dotfiles-public
RUN ./dotfiles install neovim tmux zsh

# zinit プラグインをインストールする
# ここは壊れやすくて、TERM=xterm を指定しないと失敗することがある
# TERM=xterm なら毎回成功するが、dockerfile で TERM=xterm を指定するのは嫌な感じがする
# 他のプラグインマネージャに乗り換えるかもしれない
RUN TERM=xterm zsh -i -c "zinit update --all"

RUN git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
RUN TMUX_PLUGIN_MANAGER_PATH=/home/developer/.tmux/plugins ~/.tmux/plugins/tpm/scripts/install_plugins.sh

RUN nvim --headless +Lazy! sync +qall

RUN zsh -i -c 'mise use -g node'
RUN zsh -i -c 'mise use -g python'
RUN if [ "$ENABLE_NODE" = "true" ]; then zsh -i -c 'mise use -g pnpm'; fi
RUN if [ "$ENABLE_RUST" = "true" ]; then zsh -i -c 'mise use -g "rust@stable"'; fi
RUN if [ "$ENABLE_RUST" = "true" ]; then zsh -i -c 'rustup component add rust-src rust-analyzer'; fi
RUN if [ "$ENABLE_RUST" = "true" ]; then zsh -i -c 'mise use -g "cargo:cargo-audit"'; fi
RUN if [ "$ENABLE_RUST" = "true" ]; then zsh -i -c 'mise use -g "cargo:cargo-bloat"'; fi
RUN if [ "$ENABLE_RUST" = "true" ]; then zsh -i -c 'mise use -g "cargo:cargo-edit"'; fi
RUN if [ "$ENABLE_RUST" = "true" ]; then zsh -i -c 'mise use -g "cargo:random-output"'; fi

RUN zsh -i -c 'nvim --headless +"MasonInstall json-lsp typos-lsp yaml-language-server" sync +qall'
RUN if [ "$ENABLE_C" = "true" ]; then zsh -i -c 'nvim --headless +"MasonInstall clangd" sync +qall'; fi
RUN if [ "$ENABLE_NODE" = "true" ]; then zsh -i -c 'nvim --headless +"MasonInstall astro-language-server" sync +qall'; fi
RUN if [ "$ENABLE_NODE" = "true" ]; then zsh -i -c 'nvim --headless +"MasonInstall biome" sync +qall'; fi
RUN if [ "$ENABLE_NODE" = "true" ]; then zsh -i -c 'nvim --headless +"MasonInstall html-lsp" sync +qall'; fi
RUN if [ "$ENABLE_NODE" = "true" ]; then zsh -i -c 'nvim --headless +"MasonInstall typescript-language-server" sync +qall'; fi
RUN if [ "$ENABLE_PYTHON" = "true" ]; then zsh -i -c 'nvim --headless +"MasonInstall pyright" sync +qall'; fi
RUN if [ "$ENABLE_PYTHON" = "true" ]; then zsh -i -c 'nvim --headless +"MasonInstall ruff" sync +qall'; fi

CMD ["/usr/bin/zsh"]
