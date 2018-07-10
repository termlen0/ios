# Load configuration onto device
The `load_config` function will take a Cisco IOS configuration file and load it
onto the device.  This function supports either merging the configuration with
the current active configuration or replacing the current active configuration
with the provided configuration file.  

The `load_config` function will return the full configuration diff in the
`ios_diff` fact.

NOTE: When performing a configuration replace function be sure to specify the
entire configuration to be loaded otherwise you could end up not being able to
reconnect to your IOS device after the configuration has been loaded.

## How to load and merge a configuration
Loading and merging a configuration file is the default operation for the
`load_config` function.  It will take the contents of a Cisco IOS configuration
file and merge it with the current device active configurations.

Below is an example of calling the `load_config` function from the playbook.

```
- hosts: cisco_ios

  roles:
    - name privateip.ios
      function: load_config
      src: files/ios.cfg
```

The above playbook will load the specified configuration file onto each device
in the `cisco_ios` host group.

### How to replace the current active configuration
The `load_config` function also supports replacing the current active
configuration with the configuration file located on the Ansible controller.
In order to replace the device's active configuration, set the value of the
`config_replace` setting to `True`.

```
- hosts: cisco_ios

  roles:
    - name privateip.ios
      function: load_config
      src: files/ios.cfg
      config_replace: yes
```

### Copying the new configuration to startup
When the new configuration is loaded onto the device, it is not, by default,
copied to the startup configuration.  To enable this capability in the
`load_config` function, simple set the value of `save_config` to `True`.

```
- hosts: cisco_ios

  roles:
    - name privateip.ios
      function: load_config
      src: files/ios.cfg
      save_config: yes
```

### Implement using tasks
The `load_config` function can also be implemented in the `tasks` for execution
during the play run using either the `include_role` or `import_role` modules as
shown below.

```
- hosts: cisco_ios

  tasks:
    - name: load configuration onto ios device
      import_role:
        name: privateip.ios
        tasks_from: load_config
      vars:
        src: files/ios.cfg
        config_replace: yes
```

## Arguments

### config_replace

Specifies whether or not the source configuration should replace the current
active configuration on the target IOS device.  When this value is set to
False, the source configuration is merged with the active configuration.  When
this value is set to True, the source configuration will replace the current
active configuration

The default value is `False`

### config_save

Specifies whether or not to copy the updated active configuration to the device
startup configuration.  When this value is set to `True` the configuration is
copied to the startup configuration and when this value is `False` the
configuration is not copied to the startup configuration.  Note this setting
only applies if the configuration is updated.

The default value is `False`

### source

Specifes the relative or absolute path to the device configuration to load on
to the target device.  This is a required argument for the `load_config`
function.

## Notes
None
