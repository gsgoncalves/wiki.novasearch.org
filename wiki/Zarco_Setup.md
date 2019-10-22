---
title: Zarco/Setup
permalink: wiki/Zarco/Setup/
layout: wiki
redirect_from:
 - wiki/Cluster/Setup
---


Anaconda Setup
--------------

### Install latest Anaconda distribution with Python 3.x

Set install directory to /share/apps/anaconda3/20XX.XX/

```bash
bash Anaconda3-xxxx.xx-Linux-x86_64.sh
```

Add new modulefile /share/apps/modulefiles/anaconda3/20XX.XX/

```bash
#%Module1.0
setenv PYTHONROOT /share/apps/anaconda3/20XX.XX/
prepend-path PATH /share/apps/anaconda3/20XX.XX/bin
#prepend-path LD_LIBRARY_PATH /share/apps/anaconda3/20XX.XX/lib
#prepend-path LIBPATH /share/apps/anaconda3/20XX.XX/lib
```

### JupyterHub Configuration

📖 **Repository: <https://github.com/novasearch/zarco-jupyterhub>**

Install the following dependencies to the desired base environment:

```bash
source /share/apps/anaconda3/20XX/XX/bin/activate
conda install jupyterhub
pip install git+https://github.com/jupyterhub/wrapspawner
pip install git+https://github.com/jupyterhub/batchpawner
```

Edit /share/apps/anaconda3/2019.10/lib/python3.7/site-packages/batchspawner/batchspawner.py

```diff
--- /share/apps/anaconda3/2019.10/lib/python3.7/site-packages/batchspawner/batchspawner.py.orig 2019-10-23 00:33:43.876864465 +0100
+++ /share/apps/anaconda3/2019.10/lib/python3.7/site-packages/batchspawner/batchspawner.py      2019-10-22 23:42:02.422869988 +0100
@@ -730,7 +730,7 @@
     # outputs job id string
     batch_submit_cmd = Unicode('condor_submit').tag(config=True)
     # outputs job data XML string
-    batch_query_cmd = Unicode('condor_q {job_id} -format "%s, " JobStatus -format "%s" RemoteHost -format "\n" True').tag(config=True)
+    batch_query_cmd = Unicode('condor_q {job_id} -format "%s, " JobStatus -format "%s" RemoteHost').tag(config=True)
     batch_cancel_cmd = Unicode('condor_rm {job_id}').tag(config=True)
     # job status: 1 = pending, 2 = running
     state_pending_re = Unicode(r'^1,').tag(config=True)
```

```bash
git clone git@github.com:nscr/zarco-jupyterhub.git /etc/jupyterhub
cd /etc/jupyterhub
make install
```

The last command installs the init script for supervisord. You still need to bring the service up yourself.

```bash
supervisorctl reread
supervisorctl update
```

Check the status of the service with

```bash
supervisorctl status
```

NAS
---

### Automounts

Rocks installation sets up `/share` and `/home` automount configurations. While `share` is still served by the headnode's NFS server the homedirs `/home` are served by the NAS appliance instead.

To setup the extra locations served by the external NAS appliance you can create a separate automount configuration and mount them under `/nas`.

In `/etc/auto.master` the following line was appended:

```
/nas   /etc/auto.nas   --timeout=1200
```

In  `/etc/auto.nas` setup the mounts that are needed:

```
Public  nas-0-0.fast:/Public
Datasets        nas-0-0.fast:/Datasets
wamse   nas-0-0.fast:/wamse
pmc     nas-0-0.fast:/pmc
```

Finally restart the automount daemon:

```
service autofs restart
```

### Adding the NAS as an appliance in Rocks

```bash
#!/bin/sh
#
# Add the QNAP-NAS file server to the cluster
#
echo "Add the QNAP-NAS file server to the cluster"
#rocks add host interface nas-0-0 iface=eth0 subnet=private ip=10.1.1.100 mac=00:08:9b:f6:5d:75
rocks list host | grep -q -w -e 'nas-0-0' || rocks add host nas-0-0
rocks list host interface nas-0-0 | grep -q -w -e 'eth0' || \
    rocks add host interface nas-0-0 iface=eth0
rocks set host interface subnet nas-0-0 iface=eth0 subnet=private
rocks set host interface ip     nas-0-0 iface=eth0 ip=10.1.1.100
rocks set host interface mac    nas-0-0 iface=eth0 mac=00:08:9b:f6:5d:75
#rocks add host interface nas-0-0 iface=eth0 subnet=private ip=10.1.1.100 mac=00:08:9b:f6:5d:75
rocks set host attr nas-0-0 arch x86_64
rocks set host attr nas-0-0 os linux
rocks set host attr nas-0-0 managed false
rocks set host attr nas-0-0 kickstartable no
rocks set host installaction nas-0-0 action=os
rocks set host boot nas-0-0 action=os
rocks sync config
```
