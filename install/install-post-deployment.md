#### # Begin post-deployment steps

##### # if not using LDAP, you'll need to add some htpasswd users (on all masters)
```
#htpasswd -b /etc/origin/master/htpasswd ocpadmin welcome1
```
##### # and set cluster-admin perms on admin accounts
```
#oc adm policy add-cluster-role-to-user cluster-admin ocpadmin
```
##### # setup group sync and run it once
```
wget https://raw.githubusercontent.com/nnachefski/ocpstuff/master/ocp_group_sync.conf -O /etc/origin/master/ocp_group_sync.conf
wget https://raw.githubusercontent.com/nnachefski/ocpstuff/master/ocp_group_sync-whitelist.conf -O /etc/origin/master/ocp_group_sync-whitelist.conf 
oc adm groups sync --sync-config=/etc/origin/master/ocp_group_sync.conf --confirm --whitelist=/etc/origin/master/ocp_group_sync-whitelist.conf
```
##### # set policies (perms) on your sync’ed groups
```
oc adm policy add-cluster-role-to-group cluster-admin admins
oc adm policy add-role-to-group basic-user authenticated
oc adm policy add-cluster-role-to-user cluster-reader readonly
```
##### # set your infra/masters regions to unschedulable
```
oc adm manage-node --selector=region=masters --schedulable=false
oc adm manage-node --selector=region=infra --schedulable=false
```
## # Done!

### # Now run through the rhel7-custom image build guide
#### # https://github.com/nnachefski/ocpstuff/tree/master/images
