FROM fedora
MAINTAINER “Rabi Mishra” <ramishra@redhat.com>
ENV container docker

ADD ./scripts/55-heat-config \
  /opt/stack/os-config-refresh/configure.d/

ADD ./scripts/50-heat-config-docker-compose \
  /opt/stack/os-config-refresh/configure.d/

ADD ./scripts/hooks/* \
  /var/lib/heat-config/hooks/

ADD ./scripts/heat-config-notify \
  /usr/bin/heat-config-notify

ADD ./scripts/configure_container_agent.sh /tmp/

RUN dnf -y install findutils os-collect-config os-apply-config \
  os-refresh-config dib-utils python-pip python-docker-py \
  python-yaml python-zaqarclient --setopt=tsflags=nodocs && \
  dnf clean all

#RUN dnf -y update --setopt=tsflags=nodocs && \
#  dnf -y install findutils os-collect-config os-apply-config \
#  os-refresh-config dib-utils python-pip python-docker-py \
#  python-yaml python-zaqarclient --setopt=tsflags=nodocs && \
#  dnf clean all

#RUN dnf -y update --setopt=tsflags=nodocs && dnf clean all
#
#RUN dnf -y install gcc-c++ redhat-rpm-config python27-devel findutils \
#  python-pip python-docker-py \
#  --setopt=tsflags=nodocs && \
#  dnf clean all

RUN chmod 700 /tmp/configure_container_agent.sh ; \
  bash /tmp/configure_container_agent.sh

#create volumes to share the host directories
VOLUME [ "/var/lib/cloud"]
VOLUME [ "/var/lib/heat-cfntools" ]

#set DOCKER_HOST environment variable that docker-compose would use
ENV DOCKER_HOST unix:///var/run/docker.sock

CMD /usr/bin/os-collect-config
