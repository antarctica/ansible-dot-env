# Dot ENV (`dot-env`)

**Part of the BAS Ansible Role Collection (BARC)**

Creates or modifies key-value environment file

## Overview

* An environment variable file will be created or modified to contain a collection of keys/values
* By default an example environment variable will also be created or modified to contain a collection of keys/example-values
* By default a banner message will be added to newly created environment or example environment variable files

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Usage

### Requirements

* None

### Variables

* `dot_env_file_create`
    * If "true" the environment file specified by `dot_env_file_path` will be created where it doesn't already exist
    * If "false" and the environment file doesn't exist Ansible will fail.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `dot_env_file_purge`
    * If "true" the environment file specified by `dot_env_file_path` will be deleted where it already exists
    * This is useful if you want to ensure an environment file will only contain the items you specify, otherwise these items will either update any existing items or be appended as new items.
    * It is safe for this variable to be "true" even when the file doesn't exist.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "false"
* `dot_env_file_path`
    * Path to the environment file that either already exists or should be created
    * This variable **MUST** represent a valid UNIX file path.
    * Default: "/tmp/.env"
* `dot_env_example_file_enable`
    * If "true" an example environment file will be either created or updated
    * By default this example file sits alongside the environment file and is designed to contain dummy values making the file safe to include in source control.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `dot_env_example_file_create`
    * If "true" the example environment file specified by `dot_env_example_file_path` will be created where it doesn't already exist
    * If "false" and the example environment file doesn't exist Ansible will fail.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `dot_env_example_file_purge`
    * If "true" the example environment file specified by `dot_env_example_file_path` will be deleted where it already exists
    * This is useful if you want to ensure an environment file will only contain the items you specify, otherwise these items will either update any existing items or be appended as new items.
    * It is safe for this variable to be "true" even when the file doesn't exist.
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * By default, this variable uses the value set by `dot_env_file_purge` to ensure parity between the example and environment file.
    * Default: "{{ dot_env_file_purge }}"
* `dot_env_example_file_path`
    * Path to the example environment file that either already exists or should be created
    * This variable **MUST** represent a valid UNIX file path.
    * By default, this variable uses the value set by `dot_env_file_path` so the the example and environment file sit next to each other.
    * By convention, the example file uses the "example" extension, this **SHOULD NOT** be changed.
    * Default: "{{ dot_env_file_path }}.example"
* `dot_env_file_add_banner`
    * If "true" a comment, specified by `dot_env_file_banner_message` will be added to the to top of the environment file
    * This is useful for explaining what the environment file is for and that it is generated automatically.
    * Note: The banner comment will only be added where the environment file didn't exist or has been purged. It is not currently possible to change the banner comment without purging the environment file as well. 
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * Default: "true"
* `dot_env_file_banner_message`
    * The comment to use if `dot_env_file_add_banner` is "True"
    * Blank lines can be inserted using "\n".
    * The comment symbol, `#`, automatically prepends this variable, without a trailing space.
    * Default: " Automatically managed - take care if editing this file directly to ensure changes are preserved. \n"
* `dot_env_example_file_add_banner`
    * If "true" a comment, specified by `dot_env_example_file_banner_message` will be added to the to top of the example file
    * This is useful for explaining what the environment file is for and that it is generated automatically.
    * Note: The banner comment will only be added where the example file didn't exist or has been purged. It is not currently possible to change the banner comment without purging the example file as well. 
    * This is a binary variable and **MUST** be set to either "true" or "false" (without quotes).
    * To ensure parity between the example and environment file, this variable **SHOULD** use the value set by `dot_env_file_add_banner`.
    * Default: "true"
* `dot_env_example_file_banner_message`
    * The comment to use if `dot_env_example_file_add_banner` is "True"
    * Blank lines can be inserted using "\n".
    * The comment symbol, `#`, automatically prepends this variable, without a trailing space.
    * By default, this variable uses the value set by `dot_env_file_banner_message` to ensure parity between the example and environment file.
    * Default: "{{ dot_env_file_banner_message }}"
* `dot_env_key_value_separator`
    * The symbol to use between key and value/examples
    * E.g. for a key "foo", value "bar" and separator "=" the outputted item would be "foo=bar".
    * By convention, "=" is used as a separator, this **SHOULD NOT** be changed.
    * Default: "="
* `dot_env_key_value_items`
    * The list of key, value/example items to exist within the environment or example environment file
    * Where environment or example environment file already exists and contains an item with the same key the item will be updated. Where no item with the same key exists, or a new environment or example environment file is being used, the item will be appended as a new item.
    * This variable is expressed as a collection of items with item containing the following properties:
        * `key` the name of the item or environment variable, used in both environment and example environment files
        * `value` a *real* value for this item, used in environment files only as they usually sensitive
        * `example` a *dummy* value for this item, used in example environment files only as though indicative of a *real* value it is meaningless
    * All properties for items that make up this variable **MUST** be specified, null values **SHOULD** use "" as their value.
    * Default: [] (Empty array)

## Contributing

This project welcomes contributions, see `CONTRIBUTING` for our general policy.

## Developing

### Committing changes

The [Git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/) workflow is used to manage development of this package.

Discrete changes should be made within *feature* branches, created from and merged back into *develop* (where small one-line changes may be made directly).

When ready to release a set of features/changes create a *release* branch from *develop*, update documentation as required and merge into *master* with a tagged, [semantic version](http://semver.org/) (e.g. `v1.2.3`).

After releases the *master* branch should be merged with *develop* to restart the process. High impact bugs can be addressed in *hotfix* branches, created from and merged into *master* directly (and then into *develop*).

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the BAS Web & Applications Team Jira project ([BASWEB](https://jira.ceh.ac.uk/browse/BASWEB)).

## License

Copyright 2015 NERC BAS. Licensed under the MIT license, see `LICENSE` for details.
