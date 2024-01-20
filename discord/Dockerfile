FROM debian:bookworm

LABEL description="SinusBot - Discord only image"
LABEL version="1.0.2"

# Create sinusbot user
RUN useradd --user-group --system --create-home --no-log-init sinusbot

# Install dependencies and clean up afterwards
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends ca-certificates bzip2 unzip curl python3 procps libpci3 libxslt1.1 libxkbcommon0 locales && \
    rm -rf /tmp/* /var/tmp/* /var/lib/apt/lists/*

# Set locale
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en

WORKDIR /opt/sinusbot

ADD install.sh .
RUN chmod 755 install.sh

# Download/Install SinusBot
RUN bash install.sh sinusbot

# Download/Install yt-dlp
RUN bash install.sh yt-dlp

# Download/Install Text-to-Speech
RUN bash install.sh text-to-speech

ADD entrypoint.sh .
ADD config.ini data/config.ini

RUN chmod 755 entrypoint.sh

EXPOSE 8087

RUN chown -R sinusbot:sinusbot /opt/sinusbot

VOLUME ["/opt/sinusbot/data", "/opt/sinusbot/scripts"]

ENTRYPOINT ["/opt/sinusbot/entrypoint.sh"]

HEALTHCHECK --interval=1m --timeout=10s \
  CMD curl --no-keepalive -f http://localhost:8087/api/v1/botId || exit 1

USER sinusbot

CMD /opt/sinusbot/sinusbot
