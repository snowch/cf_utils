Some python utility functions I find useful when working with notebooks (though they can work standalone too).

To see these scripts in context, take a look here: https://github.com/snowch/demo_2710

### ssh utils

```{bash}
!pip install --user --upgrade --quiet git+https://github.com/snowch/nb_utils
```
```{python}
from ssh_utils import ssh_utils
ssh = ssh_utils.SshUtil(hostname, username, password)

# scp the local_file_on_notebook to the ssh server
ssh.put('local_file_on_notebook')

# check the file was copied
ssh.cmd('ls -l local_file_on_notebook')
```

### cloudfoundry utils

```{bash}
!pip install --user --upgrade --quiet git+https://github.com/snowch/nb_utils
```
```{python}
ibm_id = ...
ibm_id_password = ...
bluemix_org_name = 'chris.snow@uk.ibm.com'
bluemix_space_name = 'dev'

#   https://api.ng.bluemix.net     - for the US South Region
#   https://api.eu-gb.bluemix.net  - for the UK
#   https://api.au-syd.bluemix.net - for Australia

target_endpoint = 'https://api.ng.bluemix.net'

from cf_utils import cf_utils
cf = cf_utils.CloudFoundryUtil(target_endpoint, ibm_id, ibm_id_password, bluemix_organization_name, bluemix_space_name)

# search the service plans for messagehub
print(cf.search_plans('message hub'))
# fe959ac5-aa47-43a6-9c58-6fc265ee9b0e

# print out all of the service plans
for plan in cf.client.service_plans.list():
    print(plan)

cf.create_service_instance(service_plan_guid, service_name_to_create, create_default_credentials = True)
cf.delete_service(service_instance_name, force=False)
```

### messagehub utils

```{bash}
!pip install --user --upgrade --quiet git+https://github.com/snowch/nb_utils
```
```{python}
ibm_id = ...
ibm_id_password = ...
bluemix_organization_name = 'chris.snow@uk.ibm.com'
bluemix_space_name = 'dev'

#   https://api.ng.bluemix.net     - for the US South Region
#   https://api.eu-gb.bluemix.net  - for the UK
#   https://api.au-syd.bluemix.net - for Australia

target_endpoint = 'https://api.ng.bluemix.net'

messagehub_instance_name = 'my_messagehub'
messagehub_topic_name = 'my_topic'

from mh_utils import mh_utils
mh = mh_utils.MessageHubUtil(target_endpoint, ibm_id, ibm_id_password, bluemix_organization_name, bluemix_space_name)

# 'fe959ac5-aa47-43a6-9c58-6fc265ee9b0e' = messagehub service guid, see cloudfoundry utils example
mh.create_service_instance('fe959ac5-aa47-43a6-9c58-6fc265ee9b0e', messagehub_instance_name)

mh.produce_message(messagehub_instance_name, messagehub_topic_name, message)
mh.consume_messages(messagehub_instance_name, messagehub_topic_name)

mh.delete_service_instance(messagehub_instance_name, True)
```
