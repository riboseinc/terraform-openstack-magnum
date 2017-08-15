# Terraform OpenStack support for Magnum

## Resources

### Magnum Cluster

Command: `openstack_container_cluster`

https://developer.openstack.org/api-ref/container-infrastructure-management/#create-new-cluster

Arguments:

* `name`, body, string, Name of the resource.
* `discovery_url`, body, string, The custom discovery url for node discovery. This is used by the COE to discover the servers that have been created to host the containers. The actual discovery mechanism varies with the COE. In some cases, Magnum fills in the server info in the discovery service. In other cases, if the discovery_url is not specified, Magnum will use the public discovery service at: https://discovery.etcd.io. In this case, Magnum will generate a unique url here for each bay and store the info for the servers.
* `master_count`, body, integer, The number of servers that will serve as master for the bay/cluster. The default is 1. Set to more than 1 master to enable High Availability. If the option master-lb-enabled is specified in the baymodel/cluster template, the master servers will be placed in a load balancer pool.
* `cluster_template_id`, body, UUID, The UUID of the cluster template.
* `node_count`, body, integer, The number of servers that will serve as node in the bay/cluster. The default is 1.
* `create_timeout`, body, integer, The timeout for cluster creation in minutes. The value expected is a positive integer and the default is 60 minutes. If the timeout is reached during cluster creation process, the operation will be aborted and the cluster status will be set to CREATE_FAILED.
* `keypair`, body, string, The name of the SSH keypair to configure in the bay/cluster servers for ssh access. Users will need the key to be able to ssh to the servers in the bay/cluster. The login name is specific to the bay/cluster driver, for example with fedora-atomic image, default login name is fedora.

```go
resource "openstack_container_cluster" "main" {
  name = "k8s"
  master_count = 2
  discovery_url = null
  master_count = 2
  cluster_template_id = "0562d357-8641-4759-8fed-8173f02c9633"
  node_count = 2
  create_timeout = 60
  keypair = "my_keypair"
}
```

Attributes:

https://developer.openstack.org/api-ref/container-infrastructure-management/#show-details-of-a-cluster

* `status`, body, string, The current state of the bay/cluster.
* `uuid`, body, UUID, The UUID of the cluster.
* `links`, body, array, Links to the resources in question.
* `stack_id`, body, UUID, The reference UUID of orchestration stack from Heat orchestration service.
* `created_at`, body, string, The date and time when the resource was created. The date and time stamp format is ISO 8601: CCYY-MM-DDThh:mm:ss±hh:mm. For example, 2015-08-27T09:49:58-05:00. The ±hh:mm value, if included, is the time zone as an offset from UTC.
* `api_address`, body, string, The endpoint URL of COE API exposed to end-users.
* `discovery_url`, body, string, The custom discovery url for node discovery. This is used by the COE to discover the servers that have been created to host the containers. The actual discovery mechanism varies with the COE. In some cases, Magnum fills in the server info in the discovery service. In other cases, if the discovery_url is not specified, Magnum will use the public discovery service at: https://discovery.etcd.io In this case, Magnum will generate a unique url here for each bay and store the info for the servers.
* `updated_at`, body, string, The date and time when the resource was updated. The date and time stamp format is ISO 8601: CCYY-MM-DDThh:mm:ss±hh:mm. For example, 2015-08-27T09:49:58-05:00. The ±hh:mm value, if included, is the time zone as an offset from UTC. In the previous example, the offset value is -05:00. If the updated_at date and time stamp is not set, its value is null.
* `master_count`, body, integer, The number of servers that will serve as master for the bay/cluster. The default is 1. Set to more than 1 master to enable High Availability. If the option master-lb-enabled is specified in the baymodel/cluster template, the master servers will be placed in a load balancer pool.
* `coe_version`, body, string, Version info of chosen COE in bay/cluster for helping client in picking the right version of client.
* `keypair`, body, string, The name of the SSH keypair to configure in the bay/cluster servers for ssh access. Users will need the key to be able to ssh to the servers in the bay/cluster. The login name is specific to the bay/cluster driver, for example with fedora-atomic image, default login name is fedora.
* `cluster_template_id`, body, UUID, The UUID of the cluster template.
* `master_addresses`, body, array, List of floating IP of all master nodes.
* `node_count`, body, integer, The number of servers that will serve as node in the bay/cluster. The default is 1.
* `node_addresses`, body, array, List of floating IP of all servers that serve as node.
* `status_reason`, body, string, The reason of bay/cluster current status.
* `create_timeout`, body, integer, The timeout for cluster creation in minutes. The value expected is a positive integer and the default is 60 minutes. If the timeout is reached during cluster creation process, the operation will be aborted and the cluster status will be set to CREATE_FAILED.
* `name`, body, string, Name of the resource.



### Magnum Cluster Template

Command: `openstack_container_cluster_template`

https://developer.openstack.org/api-ref/container-infrastructure-management#create-new-cluster-template-detail

Arguments:

* `labels` (Optional), body, array, Arbitrary labels in the form of key=value pairs. The accepted keys and valid values are defined in the bay/cluster drivers. They are used as a way to pass additional parameters that are specific to a bay/cluster driver.
* `fixed_subnet` (Optional), body, string, Fixed subnet that are using to allocate network address for nodes in bay/cluster.
* `master_flavor_id` (Optional), body, string, The flavor of the master node for this baymodel/cluster template.
* `no_proxy` (Optional), body, string, When a proxy server is used, some sites should not go through the proxy and should be accessed normally. In this case, users can specify these sites as a comma separated list of IPs. The default is None.
* `https_proxy` (Optional), body, string, The IP address for a proxy to use when direct https access from the servers to sites on the external internet is blocked. This may happen in certain countries or enterprises, and the proxy allows the servers and containers to access these sites. The format is a URL including a port number. The default is None.
* `http_proxy` (Optional), body, string, The IP address for a proxy to use when direct http access from the servers to sites on the external internet is blocked. This may happen in certain countries or enterprises, and the proxy allows the servers and containers to access these sites. The format is a URL including a port number. The default is None.
* `tls_disabled`, body, boolean, Transport Layer Security (TLS) is normally enabled to secure the bay/cluster. In some cases, users may want to disable TLS in the bay/cluster, for instance during development or to troubleshoot certain problems. Specifying this parameter will disable TLS so that users can access the COE endpoints without a certificate. The default is TLS enabled.
* `keypair_id`, body, string, The name of the SSH keypair to configure in the bay/cluster servers for ssh access. Users will need the key to be able to ssh to the servers in the bay/cluster. The login name is specific to the bay/cluster driver, for example with fedora-atomic image, default login name is fedora.
* `public`, body, boolean, Access to a baymodel/cluster template is normally limited to the admin, owner or users within the same tenant as the owners. Setting this flag makes the baymodel/cluster template public and accessible by other users. The default is not public.
* `docker_volume_size`, body, integer, The size in GB for the local storage on each server for the Docker daemon to cache the images and host the containers. Cinder volumes provide the storage. The default is 25 GB. For the devicemapper storage driver, the minimum value is 3GB. For the overlay storage driver, the minimum value is 1GB.
* `server_type`, body, string, The servers in the bay/cluster can be vm or baremetal. This parameter selects the type of server to create for the bay/cluster. The default is vm.
* `external_network_id`, body, string, The name or network ID of a Neutron network to provide connectivity to the external internet for the bay/cluster. This network must be an external network, i.e. its attribute router:external must be True. The servers in the bay/cluster will be connected to a private network and Magnum will create a router between this private network and the external network. This will allow the servers to download images, access discovery service, etc, and the containers to install packages, etc. In the opposite direction, floating IPs will be allocated from the external network to provide access from the external internet to servers and the container services hosted in the bay/cluster.
* `image_id`, body, string, The name or UUID of the base image in Glance to boot the servers for the bay/cluster. The image must have the attribute os-distro defined as appropriate for the bay/cluster driver.
* `volume_driver`, body, string, The name of a volume driver for managing the persistent storage for the containers. The functionality supported are specific to the driver.
* `registry_enabled` (Optional), body, boolean, Docker images by default are pulled from the public Docker registry, but in some cases, users may want to use a private registry. This option provides an alternative registry based on the Registry V2: Magnum will create a local registry in the bay/cluster backed by swift to host the images. The default is to use the public registry.
* `docker_storage_driver`, body, string, The name of a driver to manage the storage for the images and the container’s writable layer. The supported drivers are devicemapper and overlay. The default is devicemapper.
* `name`, body, string, Name of the resource.
* `network_driver`, body, string, The name of a network driver for providing the networks for the containers. Note that this is different and separate from the Neutron network for the bay/cluster. The operation and networking model are specific to the particular driver.
* `fixed_network` (Optional), body, string, The name or network ID of a Neutron network to provide connectivity to the internal network for the bay/cluster.
* coe, body, string, Specify the Container Orchestration Engine to use. Supported COEs include kubernetes, swarm, mesos. If your environment has additional bay/cluster drivers installed, refer to the bay/cluster driver documentation for the new COE names.
* `flavor_id`, body, string, The nova flavor ID or name for booting the node servers. The default is m1.small.
* `master_lb_enabled`, body, boolean, Since multiple masters may exist in a bay/cluster, a Neutron load balancer is created to provide the API endpoint for the bay/cluster and to direct requests to the masters. In some cases, such as when the LBaaS service is not available, this option can be set to false to create a bay/cluster without the load balancer. In this case, one of the masters will serve as the API endpoint. The default is true, i.e. to create the load balancer for the bay.
* `dns_nameserver`, body, string, The DNS nameserver for the servers and containers in the bay/cluster to use. This is configured in the private Neutron network for the bay/cluster. The default is 8.8.8.8.

```go
resource "openstack_container_cluster_template" "main" {
  no_proxy = [
    "10.0.0.0/8",
    "172.0.0.0/8",
    "192.0.0.0/8",
    "localhost"
  ]
  https_proxy = "http://10.164.177.169:8080"
  tls_disabled = false
  keypair_id = "kp"
  docker_volume_size = 3
  server_type = "vm"
  external_network_id = "public"
  image_id = "fedora-atomic-latest"
  volume_driver = "cinder"
  docker_storage_driver = "devicemapper"
  name = "k8s-bm2"
  network_driver = "flannel"
  coe = "kubernetes"
  flavor_id = "m1.small"
  master_lb_enabled = true
  dns_nameserver = "8.8.8.8"
}
```

Attributes:

https://developer.openstack.org/api-ref/container-infrastructure-management/#show-details-of-a-cluster-template-detail

* `insecure_registry`, body, string, The URL pointing to users’s own private insecure docker registry to deploy and run docker containers.
* `links`, body, array, Links to the resources in question.
* `http_proxy` (Optional), body, string, The IP address for a proxy to use when direct http access from the servers to sites on the external internet is blocked. This may happen in certain countries or enterprises, and the proxy allows the servers and containers to access these sites. The format is a URL including a port number. The default is None.
* `updated_at`, body, string, The date and time when the resource was updated, The date and time stamp format is ISO 8601: CCYY-MM-DDThh:mm:ss±hh:mm For example, 2015-08-27T09:49:58-05:00. The ±hh:mm value, if included, is the time zone as an offset from UTC. In the previous example, the offset value is -05:00. If the updated_at date and time stamp is not set, its value is null.
* `floating_ip_enabled`, body, boolean, Whether enable or not using the floating IP of cloud provider. Some cloud providers used floating IP, some used public IP, thus Magnum provide this option for specifying the choice of using floating IP.
* `fixed_subnet` (Optional), body, string, Fixed subnet that are using to allocate network address for nodes in bay/cluster.
* `master_flavor_id` (Optional), body, string, The flavor of the master node for this baymodel/cluster template.
* `uuid`, body, UUID, The UUID of the cluster template.
* `no_proxy` (Optional), body, string, When a proxy server is used, some sites should not go through the proxy and should be accessed normally. In this case, users can specify these sites as a comma separated list of IPs. The default is None.
* `https_proxy` (Optional), body, string, The IP address for a proxy to use when direct https access from the servers to sites on the external internet is blocked. This may happen in certain countries or enterprises, and the proxy allows the servers and containers to access these sites. The format is a URL including a port number. The default is None.
* `tls_disabled`, body, boolean, Transport Layer Security (TLS) is normally enabled to secure the bay/cluster. In some cases, users may want to disable TLS in the bay/cluster, for instance during development or to troubleshoot certain problems. Specifying this parameter will disable TLS so that users can access the COE endpoints without a certificate. The default is TLS enabled.
* `keypair_id`, body, string, The name of the SSH keypair to configure in the bay/cluster servers for ssh access. Users will need the key to be able to ssh to the servers in the bay/cluster. The login name is specific to the bay/cluster driver, for example with fedora-atomic image, default login name is fedora.
* `public`, body, boolean, Access to a baymodel/cluster template is normally limited to the admin, owner or users within the same tenant as the owners. Setting this flag makes the baymodel/cluster template public and accessible by other users. The default is not public.
* `labels` (Optional), body, array, Arbitrary labels in the form of key=value pairs. The accepted keys and valid values are defined in the bay/cluster drivers. They are used as a way to pass additional parameters that are specific to a bay/cluster driver.
* `docker_volume_size`, body, integer, The size in GB for the local storage on each server for the Docker daemon to cache the images and host the containers. Cinder volumes provide the storage. The default is 25 GB. For the devicemapper storage driver, the minimum value is 3GB. For the overlay storage driver, the minimum value is 1GB.
* `server_type`, body, string, The servers in the bay/cluster can be vm or baremetal. This parameter selects the type of server to create for the bay/cluster. The default is vm.
* `external_network_id`, body, string, The name or network ID of a Neutron network to provide connectivity to the external internet for the bay/cluster. This network must be an external network, i.e. its attribute router:external must be True. The servers in the bay/cluster will be connected to a private network and Magnum will create a router between this private network and the external network. This will allow the servers to download images, access discovery service, etc, and the containers to install packages, etc. In the opposite direction, floating IPs will be allocated from the external network to provide access from the external internet to servers and the container services hosted in the bay/cluster.
* `cluster_distro`, body, string, Display the attribute os-distro defined as appropriate metadata in image for the bay/cluster driver.
* `image_id`, body, string, The name or UUID of the base image in Glance to boot the servers for the bay/cluster. The image must have the attribute os-distro defined as appropriate for the bay/cluster driver.
* `volume_driver`, body, string, The name of a volume driver for managing the persistent storage for the containers. The functionality supported are specific to the driver.
* `registry_enabled` (Optional), body, boolean, Docker images by default are pulled from the public Docker registry, but in some cases, users may want to use a private registry. This option provides an alternative registry based on the Registry V2: Magnum will create a local registry in the bay/cluster backed by swift to host the images. The default is to use the public registry.
* `docker_storage_driver`, body, string, The name of a driver to manage the storage for the images and the container’s writable layer. The supported drivers are devicemapper and overlay. The default is devicemapper.
* `apiserver_port`, body, integer, The exposed port of COE API server.
* `name`, body, string, Name of the resource.
* `created_at`, body, string, The date and time when the resource was created, The date and time stamp format is ISO 8601: CCYY-MM-DDThh:mm:ss±hh:mm. For example, 2015-08-27T09:49:58-05:00. The ±hh:mm value, if included, is the time zone as an offset from UTC.
* `network_driver`, body, string, The name of a network driver for providing the networks for the containers. Note that this is different and separate from the Neutron network for the bay/cluster. The operation and networking model are specific to the particular driver.
* `fixed_network` (Optional), body, string, The name or network ID of a Neutron network to provide connectivity to the internal network for the bay/cluster.
* `coe`, body, string, Specify the Container Orchestration Engine to use. Supported COEs include kubernetes, swarm, mesos. If your environment has additional bay/cluster drivers installed, refer to the bay/cluster driver documentation for the new COE names.
* `flavor_id`, body, string, The nova flavor ID or name for booting the node servers. The default is m1.small.
* `master_lb_enabled`, body, boolean, Since multiple masters may exist in a bay/cluster, a Neutron load balancer is created to provide the API endpoint for the bay/cluster and to direct requests to the masters. In some cases, such as when the LBaaS service is not available, this option can be set to false to create a bay/cluster without the load balancer. In this case, one of the masters will serve as the API endpoint. The default is true, i.e. to create the load balancer for the bay.
* `dns_nameserver`, body, string, The DNS nameserver for the servers and containers in the bay/cluster to use. This is configured in the private Neutron network for the bay/cluster. The default is 8.8.8.8.


### Magnum Quota

Command: `openstack_quota`

```go
openstack_quota "main" {
   project_id =  "aa5436ab58144c768ca4e9d2e9f5c3b2"
   resource =  "${openstack_cluster.main.name}"
   hard_limit =  10
}
```

Arguments:

* `project_id` ID of project
* `resource` the Magnum cluster resource name
* `hard_limit`

