FROM gcr.io/google-appengine/debian9
ENV KIBANA_VERSION 6.7.2
ENV KIBANA_SHA512 71b89f326374319d526e7fa6af916463333990c3723150f5cc55450a239adc6f4202e9fb9924e09accd7e956721c2795c1652713cd784da3f78bf26ac9dc47bb

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
