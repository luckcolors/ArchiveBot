FROM debian:stretch
LABEL maintainer="https://github.com/luckcolors"

RUN apt update && \
    apt-get install -y \
      wget rsync build-essential python3-dev python3-pip \
      libxml2-dev libxslt-dev zlib1g-dev libssl-dev libsqlite3-dev \
      libffi-dev git fontconfig-config fonts-dejavu-core \
      libfontconfig1 libfreetype6 libjpeg62-turbo libjpeg62-turbo-dev \
      libssl1.0.2 libssl-dev lsof ffmpeg autossh \
#      Those are here in case there's the need to work inside the container
      grep sed nano vim && \
    rm -rf /var/lib/apt/lists/*

ENV PHANTOMJS_VER 2.1.1
ENV PHANTOMJS_ARCH x86_64

RUN wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-$PHANTOMJS_VER-linux-$PHANTOMJS_ARCH.tar.bz2 && \
    bzip2 -d phantomjs-$PHANTOMJS_VER-linux-$PHANTOMJS_ARCH.tar.bz2 && \
    tar -xvf phantomjs-$PHANTOMJS_VER-linux-$PHANTOMJS_ARCH.tar && \
    cp phantomjs-$PHANTOMJS_VER-linux-$PHANTOMJS_ARCH/bin/phantomjs /usr/bin/phantomjs && \
#    This wget command is already in place to support the new phantomjs repo location
#    wget https://github.com/ariya/phantomjs/releases/download/$PHANTOMJS_VER/phantomjs -O /usr/bin/phantomjs && \
    chmod a+x /usr/bin/phantomjs && \
    rm -r phantomjs-$PHANTOMJS_VER-linux-$PHANTOMJS_ARCH && \
    rm phantomjs-$PHANTOMJS_VER-linux-$PHANTOMJS_ARCH.tar

ENV PATH=/root/.local/bin:$PATH

RUN pip3 install --upgrade pip && \
    git clone https://github.com/ArchiveTeam/ArchiveBot /ArchiveBot && \
    cd /ArchiveBot && \
    git submodule update --init && \
    pip3 install --user -r pipeline/requirements.txt && \
    mkdir -p /ArchiveBot/pipeline

WORKDIR /ArchiveBot/pipeline

ENV RSYNC_URL=rsync://fos.textfiles.com/archivebot/
ENV REDIS_URL=redis://127.0.0.1:16379/0
ENV FINISHED_WARCS_DIR=/warcs4fos
ENV NO_SCREEN=1

CMD /bin/bash
