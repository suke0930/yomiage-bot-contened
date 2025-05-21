#RVC
FROM ubuntu:latest

# 必要なパッケージをrootでインストール
RUN apt-get update && \
    apt-get install -y \
    curl git make build-essential libssl-dev zlib1g-dev \
    libbz2-dev libreadline-dev libsqlite3-dev wget llvm \
    libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
    libffi-dev liblzma-dev ffmpeg sudo && \
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

# 仮想環境を使ってプロジェクトをクローン＆依存関係インストール
# （ここでは例としてGitHubの任意プロジェクトを指定）
RUN git clone https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI.git
# COPY requirements.txt /root/Retrieval-based-Voice-Conversion-WebUI/

RUN bash -lc "source ~/venv/bin/activate &&\
python -m pip install --upgrade pip==23.3.1" 
# RUN bash -lc "source ~/venv/bin/activate &&\
# pip -V"
# RUN bash -lc "source ~/venv/bin/activate &&\
#     cd ~/Retrieval-based-Voice-Conversion-WebUI && \
#     pip install omegaconf==2.0.6 "

RUN bash -lc " source ~/venv/bin/activate &&\
    cd ~/Retrieval-based-Voice-Conversion-WebUI && \
    pip install -r requirements.txt"
    RUN bash -lc "source ~/venv/bin/activate &&\
   pip uninstall torch -y &&\
   pip install torch==2.5.1"

    RUN bash -lc " source ~/venv/bin/activate &&\
    cd ~/Retrieval-based-Voice-Conversion-WebUI && \
    wget -P assets/hubert https://huggingface.co/lj1995/VoiceConversionWebUI/resolve/main/hubert_base.pt"
# 作業ディレクトリをプロジェクトに変更し、仮想環境を有効にした状態で起動
WORKDIR /root/Retrieval-based-Voice-Conversion-WebUI
#=`` ENTRYPOINT ["tail", "-F", "/dev/null"]
CMD ["/bin/bash", "-l", "-c", "source ~/venv/bin/activate && python3 infer-web.py"]
