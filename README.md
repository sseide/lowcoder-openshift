# lowcoder-openshift
Updates for LowCoder to getit running inside OpenShift Container Platform

## All-In-One Image

### Changes needed to current lowcoder v2.3 release to run inside Openshift

1. Dockerfile
   1. add runtime user "lowcoder" to group "root"
   2. create "/lowcoder-stacks" directory and make it group "root" writable
   3. same for "api-service/logs" directory (why in the first place does it write logs into the container?)
   4. make group "root" writable "/lowcoder/etc/supervisord"
   5. make group "root" writable multiple different nginx directories
2. main entrypoint.sh
   1. no call to usermod command (not modify runtime user)
   2. no chown of /lowcoder-stacks, already done in Dockerfile
   3. modify all supervisord program configs to remove "user=" directive as user switch not allowed
3. api-service/entrypoint.sh
   1. do not use "gosu" as we are already running non-privileged
4. node-service/entrypoint.sh
    1. do not use "gosu" as we are already running non-privileged
5. supervisord.conf
   1. disable inet server socket (not used)
   2. set "logfile_maxbytes = 0" to disable log rotation on stocket
