FROM marketplace.gcr.io/google/debian9
ENV KIBANA_VERSION 7.9.0
ENV KIBANA_SHA512 aee27bfa11b1a513c2db9400e884ea17fd7bc5d97633f02ba48e2e678e60191c318b9c333ec6a9f8b276284ee4933738f83c27545c4c744e76895478e9c92404

ENV C2D_RELEASE 7.9.0

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
