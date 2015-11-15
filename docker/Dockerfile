FROM jdubois/jhipster-docker
MAINTAINER Pascal Grimaud <pascalgrimaud@gmail.com>

# parameters
ENV JHIPSTER_PROJECT sampleElasticSearch
ENV JHIPSTER_GITHUB_URL https://github.com/pascalgrimaud/jhipster-sample-app-elasticsearch.git
ENV JHIPSTER_GITHUB_NAME jhipster-sample-app-elasticsearch

# clone project
USER jhipster
RUN cd /home/jhipster && \
    git clone ${JHIPSTER_GITHUB_URL} && \
    cd /home/jhipster/${JHIPSTER_GITHUB_NAME} && \
    mvn -Pprod package -DskipTests=true && \
    mv /home/jhipster/${JHIPSTER_GITHUB_NAME}/target/*.war /home/jhipster/app.war && \
    rm -Rf /home/jhipster/${JHIPSTER_GITHUB_NAME} && \
    rm -Rf /home/jhipster/.m2/repository && \
    rm -Rf /home/jhipster/.npm/

RUN echo '#!/bin/sh' > /home/jhipster/run.sh
RUN echo 'echo "The application will start in 20sec..." && sleep 20' >> /home/jhipster/run.sh
RUN echo 'java -jar /home/jhipster/app.war \\' >> /home/jhipster/run.sh
RUN echo '--spring.profiles.active=prod \\' >> /home/jhipster/run.sh
RUN echo '--spring.datasource.url="jdbc:mysql://${MYSQL_PORT_3306_TCP_ADDR}:${MYSQL_PORT_3306_TCP_PORT}/${JHIPSTER_PROJECT}?useUnicode=true&characterEncoding=utf8" \\' >> /home/jhipster/run.sh
RUN echo '--spring.data.elasticsearch.cluster-nodes="${ELASTIC_PORT_9300_TCP_ADDR}:${ELASTIC_PORT_9300_TCP_PORT}"' >> /home/jhipster/run.sh

RUN chmod +x /home/jhipster/run.sh
RUN ls -al /home/jhipster

EXPOSE 8080
CMD ["/home/jhipster/run.sh"]
