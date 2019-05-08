FROM gcr.io/google-appengine/debian9
ENV KIBANA_VERSION 7.0.1
ENV KIBANA_SHA512 d085d85e988f8c008e6f2a528683d5336e7b263ff0e5615fcf55611c44702c6eaafad470c5bd3cf43b1c580e705a2595db80b43f61b4168efcd09600d3d1ce34

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
