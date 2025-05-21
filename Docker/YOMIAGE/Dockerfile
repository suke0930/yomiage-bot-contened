#YOMIAGE
FROM ubuntu:latest

# 必要なパッケージをrootでインストール
RUN apt-get update && \
    apt-get install -y \
    curl git make build-essential libssl-dev zlib1g-dev \
    libbz2-dev libreadline-dev libsqlite3-dev wget llvm \
    libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
    libffi-dev ffmpeg liblzma-dev sudo && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


# pyenv環境変数の設定
ENV PYENV_ROOT="/root/.pyenv"
ENV PATH="$PYENV_ROOT/bin:$PYENV_ROOT/shims:$PATH"


WORKDIR /root

# pyenvインストールと初期化設定
RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv && \
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc && \
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc && \
    echo 'eval "$(pyenv init --path)"' >> ~/.bashrc && \
    echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# pyenvでPython 3.10インストール＆仮想環境作成
RUN bash -lc "\
    pyenv install 3.10 && \
    pyenv global 3.10 && \
    pip install --upgrade pip && \
    pip install virtualenv && \
    virtualenv ~/venv"

RUN echo ""
RUN git clone https://github.com/suke0930/yomiage-bot-contened.git
COPY requirements.txt /root/yomiage-bot-contened/
RUN bash -lc " source ~/venv/bin/activate &&\
    cd yomiage-bot-contened &&\
    pip install -r requirements.txt"
WORKDIR /root/yomiage-bot-contened/


# ENTRYPOINT ["tail", "-F", "/dev/null"]
CMD ["/bin/bash", "-l", "-c", "source ~/venv/bin/activate && python3 main.py"]