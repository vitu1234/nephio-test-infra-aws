### **OAI 5G CONFIGURATION**

  Below is the overall IP addresses configured on the clusters

  ![image.png](../images/images_bootstrap_5g_guide/image%2044.png)

  Attach Extra interfaces to each worker node in each workload cluster and each of the interfaces has to achieve the following IP addresses for this demo

- 4 extra Interfaces to the edge cluster – edge worker node:

  ![image.png](../images/images_bootstrap_5g_guide/image%2045.png)
- 2 extra Interfaces to the core cluster – core worker node:

  ![image.png](../images/images_bootstrap_5g_guide/image%2046.png)
- 3 extra Interfaces to the regional cluster –regional worker node:

  ![image.png](../images/images_bootstrap_5g_guide/image%2047.png)
- Ssh into the bastion host then into each worker node in each workload cluster and install gtp module

  In the test scenario, using the Nephio Topology as shown in the diagram below

  ![image.png](../images/images_bootstrap_5g_guide/image%2048.png)

  I configured the following network and interfaces for my clusters - interfaces:

  Install gtp5g module on edge clusters all worker nodes

```bash
  sudo su
  git clone https://github.com/free5gc/gtp5g.git
  cd gtp5g
  make
  make install
```

  After attaching the interfaces to the worker nodes as shown in the picture above and no IP address is added to the interface, run this command to force DHCP resolve the client:

```bash
  #try this 
  sudo dhclient
  #or
  sudo dhclient <interface-namesip>
```

  after the above command, IP routes will change, do the following to revert to original routes

  As a example on the edge cluster, delete conflicting routes. Do the same procedure on the other clusters

  Since `ens11` also has a `192.168.1.x` address, remove its conflicting route:

```bash

  sudo ip route del 192.168.1.0/24 dev ens11

```

  This prevents traffic from using `ens11` instead of `ens3`.

  **Test the instances if they can ping each other and proceed**

  In the management cluster clone the following repos

```bash
  git clone https://github.com/vitu1234/oai-packages.git
  git clone https://github.com/vitu1234/catalog.git
  git clone https://github.com/vitu1234/nephio-test-infra-openstack

  mv nephio-test-infra-openstack/ test-infra
```

  Follow the official guide https://github.com/nephio-project/docs/blob/main/content/en/docs/guides/user-guides/exercise-2-oai.md from step 3

  For testing purposes, the following was used for configuring interfaces on nodes. These configuration should match what is in oai-packages/oai-* nad-*.yaml files

  ![5G ip addresses image.png](../images/images_bootstrap_5g_guide/5G_ip_addresses_image.png)

  On step 5 of the upstream documentation there is a note below:

> *All the NFs will wait for NRF to come up and then they will register to NRF. SMF has a dependency on UPF which is described by dependency.yaml file in SMF package. It will wait till the time UPF is deployed. It takes around ~800 seconds for the whole core network to come up. NRF is exposing its service via MetalLB external ip-address. In case MetalLB ip-address pool is not properly defined in the previous section, then UPF will not be able to register to NRF and in this case SMF and UPF will not be able to communicate.*

  The nrf and udr requires a LoadBalancer Service. In the test setup, I configured in oai-cp-operators Deployments in oai-udr-operator and oai-nrf-operator. Changing the environment variable env: SVC_TYPE = LoadBalancer and put the LoadBalancer in it directly

  Get the LBA IP address and put it for the UPF in **oai-up-operators in the edge repo ConfigMap oai-upf-op-conf and put the NodePort IP address for NRF and the port the service is running on.**

```yaml
      fqdn:
        nrf: 'nrf-service-address-here'
        //example  nrf: '192.168.28.148'
```

  **BONUS COMMANDS IN CASE THE UI IS  NOT WORKING AND WANT TO EDIT PUBLISHED PACKAGES**

```bash
  porchctl rpkg copy -n default core-75b3994efd9406140903f2433c2289ad8ceb0a86 --workspace=v2
  ## WE CAN SEE THE DRAFT VERSION, CLONE IT AND EDIT
  porchctl rpkg get --name oai-amf
  ## pull in the local directory
  **porchctl rpkg pull core-f0cdd017cb2ca35fb48955fb3ae68fa033efca0e -ndefault oai-amf
  ##Push the package
   porchctl rpkg push core-f0cdd017cb2ca35fb48955fb3ae68fa033efca0e -ndefault oai-amf
  # List package revisions to identify relevant drafts:**
  porchctl rpkg get --name oai-amf
  # Propose draft package revisions to be be published
  porchctl rpkg propose core-f0cdd017cb2ca35fb48955fb3ae68fa033efca0e -ndefault
  # Confirm the package revisions are now Proposed
  porchctl rpkg get --name oai-amf
  # Approve a proposal to publish a package revision
  porchctl rpkg approve core-f0cdd017cb2ca35fb48955fb3ae68fa033efca0e -ndefault

```

  **Hint: If using with cilium CNI, set the following to false] ALL NODES**

```bash
    kubectl edit cm -n kube-system cilium-config  --kubeconfig core.kubeconfig

    # -- Make Cilium take ownership over the `/etc/cni/net.d` directory on the
    # node, renaming all non-Cilium CNI configurations to `*.cilium_bak`.
    # This ensures no Pods can be scheduled using other CNI plugins during Cilium
    # agent downtime.
    exclusive: true
```

  Restart daemonsets for multus and cilium

```bash
  kubectl rollout restart ds cilium kube-multus-ds  -n kube-system --kubeconfig core.kubeconfig
```
