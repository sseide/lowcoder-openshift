FROM lowcoderorg/lowcoder-ce:2.3

USER 0

COPY docker-files/ /lowcoder/

# updates needed to get this image running inside RedHat OpenShift Container Platform
# Openshift starts all container as non-rooot with random user id putting user inside the "root" group
# to allow file system write access for specific folders
#
# Things to do:
# - pre-create "/lowcoder-stacks" directory, writable for user/group
# - make api-service log dir writable
# - make supervisor config dir writeable (needed for copy conf-availabe to conf-enabled)
# - modifications for nginx, some writeble directories needed too

RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update -q \
  && apt-get upgrade -q -y \
  && echo "\n=== Fixes for OpenShift compatibility ==================================\n" \
  && adduser lowcoder root \
  && mkdir -p /lowcoder-stacks \
  && chmod -R g+rw /lowcoder-stacks \
  && chown -R lowcoder:root /lowcoder-stacks \
  && chmod -R g+w /lowcoder/api-service/logs \
  && chown -R lowcoder:root /lowcoder/api-service/logs \
  && chmod -R g+w /lowcoder/etc/supervisord \
  && chown -R lowcoder:root /lowcoder/etc/supervisord \
  && chmod -R g+rw /etc/nginx \
  && chown -R lowcoder:root /var/log \
  && chmod -R g+rw /var/log /run /var/cache/nginx/ \
  && echo "\n=== Cleanup ============================================================\n" \
  && apt-get clean \
  && rm -rf /tmp/* /var/lib/apt/lists/*

#USER 9001
