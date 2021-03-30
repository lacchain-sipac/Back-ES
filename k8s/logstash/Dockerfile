FROM logstash:6.4.0
COPY logstash.conf /usr/share/logstash/pipeline/
CMD ["-f", "/usr/share/logstash/pipeline/logstash.conf"]

#docker build -t proyectocadena/logstash:6.4.0 -f Dockerfile .
# docker push proyectocadena/logstash:6.4.0
