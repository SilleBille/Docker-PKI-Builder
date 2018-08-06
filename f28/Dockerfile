FROM registry.fedoraproject.org/fedora:28
LABEL \
    name="dmoluguw/pki-maintainer:f28" \
    maintainer="Dinesh Prasanth M K <dmoluguw@redhat.com>" 

# PYTHONCOERCECLOCALE warning
ENV LC_ALL=C.UTF-8

# Install required packages
RUN dnf install -y git dnf-plugins-core vim rpm-build && \
    dnf copr enable -y @pki/10.6

# Update your base image without conflicts
RUN dnf -y update --best --allowerasing \
    && dnf clean all

# To avoid RPM errors
RUN ls /var/lib/rpm/ \
    && mv /var/lib/rpm/__db* /tmp/

# Install `fedpkg` and `bodhi-client`
RUN dnf install -y fedpkg bodhi-client

# build env
ENV BUILDDIR="/tmp/workdir" \
    BUILDUSER="builduser" \
    BUILDUSER_UID=1000 \
    BUILDUSER_GID=1000
VOLUME ["${BUILDDIR}"]
EXPOSE 389 8080 8443

VOLUME [ "/sys/fs/cgroup", "/tmp", "/run" ]
STOPSIGNAL SIGRTMIN+3
ENTRYPOINT [ "/usr/sbin/init" ]
