Role Name
=========

A utility role to help with terraform provider binaries collection for installation in airgap/disconnected environment.
This role contains a set of tasks that can be used to download the source from each of the provider in the provided list, build the go binary for each and create a bundle that can then be moved to the disconnected environment and used in that environment. 
The default behavior is to run this on a connected machine to do the collection. Once the bundle is created and moved to the airgap environemnt along with a copy of this repository a couple of tasks files are provided to unpack and install the bundle as well as configure a default provider if necessary. This has been tested on v012 of terraform and for the ovirt provider but should work for other providers included with minor adjustment for the default configuration of said provider.

Requirements
------------


Role Variables
--------------

- ver_terraform_package: The version of terraform to download and install on the connected host. Default to "0.12.30".
- ansible_module_name: The name of this module to provide context when this is run as part of a larger playbook. Default to "terraform-provider-binary-collector"
- config_default_terraform_provider: Whether to configure a default provider or not, only used on the disconnected host. Default to "true".
- install_terraform_provider: Whether to install the provider or not, only used on the disconnected host. Default to "true".
- install_terraform_provider_bundle: The path to the bundle when creating it or when installing it. Default to "/tmp/tf-providers-bundle.tar.gz"
- terraform_client_binary: The name of the terraform client binary to to installed. Default to "/tmp/tmerraform-provider-ovirt".
- tf_dir_bundle_location: The directory where the bundle is created on the connected host. Default to "/tmp".
- local_home: The home of the ansible user running the playbook, used to configure golang. Default to  "{{ lookup('env', 'HOME') }}". 
- terraform_provider_dir: The plugin installation directory on the disconnected host for terraform provider binaries. Default to "{{ local_home }}/.terraform.d/plugins".
- terraform_provider_config_file: The terraform rc file created for the default provider on the disconnected host. Default to "{{ local_home }}/.terraformrc".
- terraform_provider_full_name: The name of the default provider to configure on the disconnected host. Default to "terraform-provider-ovirt".
- terraform_domain: The name of the domain to use for the local terraform registry configuration on the disconnected host. Default to "myofflineregistry". 
- terraform_namespace: The name of the namespace to use for the local terraform registry configuration on the disconnected host. Default to "dev".
- terraform_provider_type: The architecture of the provider being installed on the disconnected host. Default to "linux_amd64".
- terraform_providers: The structure containing information about each of the providers to be installed on the disconnected host. Has a key for each provider (aws, azurerm, vsphere, ovirt, openstack, google, ibm). Each key contains the version attribute for the version being installed and if a default is being configured then a default attribute is set to true.

Dependencies
------------


Example Playbook
----------------

To run this role to collect and build terraform binaries for providers defined in the default/main.yml file use the following ( Note that the name of the role should match the name of your clone of the repo or how it is named in your requirements.yml):

    - hosts: localhost
      tasks:
         - name: Import terraform providers bundle collection tasks
           import_role:
             name: configure-terraform-for-disconnected
             tasks_from: collect-tf-binaries-bundle.yml

To run this role on the disconnected host to extract, install and configure the providers, use the following ( Note that the name of the role should match the name of your clone of the repo or how it is named in your requirements.yml):

    - hosts: localhost
      roles:
         - { role: configure-terraform-for-disconnected }


To run this role to install terraform on the connected machine in order to start the collection process use the following ( Note that the name of the role should match the name of your clone of the repo or how it is named in your requirements.yml):

    - hosts: localhost
      tasks:
         - name: Import terraform installation tasks
           import_role:
             name: configure-terraform-for-disconnected
             tasks_from: install-terraform-connected.yml


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
