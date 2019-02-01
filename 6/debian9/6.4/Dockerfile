FROM gcr.io/google-appengine/debian9
ENV KIBANA_VERSION 6.4.3
ENV KIBANA_SHA512 db55d5201675d1d71cbc511ffd4fb94332316eef8f06c2419d498b930e068030c625c18a0c2fec761cb0c066abc16ddf2c2d8a2fa59aaa3bc35ea1e289b8ae99

ARG KIBANA_URL="https://artifacts.elastic.co/downloads/kibana/kibana-oss-$KIBANA_VERSION-linux-x86_64.tar.gz"

WORKDIR /usr/share/kibana

RUN set -x && \
    apt-get update && apt-get install -qq -y fontconfig wget libfreetype6 && \
    wget -O kibana-oss.tar.gz "$KIBANA_URL" && \
    echo "$KIBANA_SHA512" && \
    echo "$KIBANA_SHA512 kibana-oss.tar.gz" | sha512sum -c - && \
    tar -zxf kibana-oss.tar.gz --strip-components=1 && \
    ln -s /usr/share/kibana /opt/kibana && \
    chown -R 1000:0 . && \
    chmod -R g=u /usr/share/kibana && \
    find /usr/share/kibana -type d -exec chmod g+s {} \;

ENV ELASTIC_CONTAINER true
ENV PATH=/usr/share/kibana/bin:$PATH

COPY --chown=1000:0 kibana-oss.yml /usr/share/kibana/config/kibana.yml
COPY --chown=1000:0 docker-entrypoint.sh /usr/local/bin/

RUN find /usr/share/kibana -gid 0 -and -not -perm /g+w -exec chmod g+w {} \; && \
    groupadd --gid 1000 kibana && \
    useradd --uid 1000 --gid 1000 \
      --home-dir /usr/share/kibana --no-create-home \
      kibana

EXPOSE 5601
USER kibana
CMD ["/usr/local/bin/docker-entrypoint.sh"]
