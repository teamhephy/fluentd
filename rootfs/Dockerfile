FROM fluent/fluentd:v1.3

COPY fluentd /fluentd
COPY entrypoint.sh /bin/
RUN chmod +x /bin/entrypoint.sh

RUN apk add bash findutils
RUN apk add --update --virtual .build-deps sudo build-base ruby-dev \
    &&  gem install bundler \
    &&  bundle config --global silence_root_warning 1 \
    &&  bundle install --gemfile=/fluentd/deis-output/Gemfile \
    &&  rake --rakefile=/fluentd/deis-output/Rakefile build \
    &&  fluent-gem install --no-document fluent-plugin-kubernetes_metadata_filter -v 2.1.4 \
    &&  fluent-gem install --no-document fluent-plugin-elasticsearch -v 4.0.4 \
    &&  fluent-gem install --no-document fluent-plugin-remote_syslog -v 1.0.0 \
    &&  fluent-gem install --no-document fluent-plugin-sumologic-mattk42 -v 0.0.4 \
    &&  fluent-gem install --no-document fluent-plugin-gelf-hs -v 1.0.7 \
    &&  fluent-gem install --no-document influxdb -v 0.6.1 \
    &&  fluent-gem install --no-document nsq-ruby -v 2.3.1 \
    &&  fluent-gem install --local /fluentd/deis-output/pkg/fluent-plugin-deis_output-0.1.0.gem \
    &&  sudo gem sources --clear-all \
    &&  apk del .build-deps \
    &&  rm -rf /var/cache/apk/* /root/.gem/ruby/*/cache/*.gem

ENTRYPOINT ["/bin/entrypoint.sh"]

CMD ["/fluentd/sbin/boot"]
