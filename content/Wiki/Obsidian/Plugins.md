---
tags:
  - obsidian
  - notetaking
---
An automatically generated list of Obisidan Plugins that I use:

```
import os
import json
import re


def get_value_from_json(file_path, key):
    with open(file_path, 'r') as json_file:
        data = json.load(json_file)
    return data.get(key, None)

community_plugin_folder_path = os.path.join(@vault_path, ".obsidian/plugins")
core_plugins_file_path =os.path.join(@vault_path, ".obsidian/core-plugins-migration.json")
community_plugins_file_path = os.path.join(@vault_path, ".obsidian/community-plugins.json")
   



plugin_folders = [name for name in os.listdir(community_plugin_folder_path) if os.path.isdir(os.path.join(plugin_path, name))]

#read in list of core plugins and activation state
with open(core_plugins_file_path, 'r') as f:
    core_plugins_dict = json.load(f)
    
#read in list of enabled community plugins
with open(community_plugins_file_path, 'r') as f:
    community_plugins_list = json.load(f)


key_name = 'name'
key_id = 'id'

community_plugin_name_list = list()

for plugin in plugin_folders:
	file_path = plugin_path + "/" + plugin + "/manifest.json"
	community_plugin_name_list.append((get_value_from_json(file_path, key_name),get_value_from_json(file_path, key_id)))


#split into enabled and not enabled lists. Do both core and community plugins, add (core) or (community) to end for tracking in name
enabled_plugins = list()
disabled_plugins = list()


for key, value in core_plugins_dict.items():

    key += " (core)"
    

    if value:
        enabled_plugins.append(key)
    else:
        disabled_plugins.append(key)


for plugin_name, plugin_id in community_plugin_name_list:
	plugin_name += " (community)"
	
	if plugin_id in community_plugins_list:
		enabled_plugins.append(plugin_name)
	else:
		disabled_plugins.append(plugin_name)






#output list of enabled plugins
print(f"Enabled Plugins ")
enabled_plugins.sort()
for name in enabled_plugins:
	name = re.sub(r'[\/\\<>"|?*]', '-', name)
	print(f"- [[{name}]]: ")

#output list of disabled plugins
print(f"Disabled Plugins ")
disabled_plugins.sort()
for name in disabled_plugins:
	name = re.sub(r'[\/\\<>"|?*]', '-', name)
	print(f"- [[{name}]]: ")
```