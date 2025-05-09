### **STEP BY STEP PICTURE VIEW GUIDE**

#### Step 1

- Create and Configure AWS Resources

  ![create-vpc.jpg](../images/images_bootstrap_5g_guide/create-vpc.jpg)
- After creating you should have something like this

  ![image.png](../images/images_bootstrap_5g_guide/image%204.png)

  ![overview-resources-after-vpc-creation.jpg](../images/images_bootstrap_5g_guide/overview-resources-after-vpc-creation.jpg)
- Edit subnet settings to allow auto-assign public ipv4 address to instances

  ![public-subnet-settings.jpg](../images/images_bootstrap_5g_guide/public-subnet-settings.jpg)
- create you key-pair in EC2 menu

  ![create-key-pair-download.jpg](../images/images_bootstrap_5g_guide/create-key-pair-download.jpg)

#### Step 2

- Configure Ansible Playbooks with Values from the created AWS resources

  - Variables to create management cluster Instances:

  ![image.png](../images/images_bootstrap_5g_guide/image%205.png)
- - Auth Variables for Ansible to create AWS EC2 Instances:

  ![image.png](../images/images_bootstrap_5g_guide/image%206.png)

  - Auth Variables for cloud formation and Cluster API:

    ![image.png](../images/images_bootstrap_5g_guide/image%207.png)
- Auth Variables expire after 12hours, can be updated later!

#### Step 3

- Run the Ansible Playbook program from the local computer

  ![image.png](../images/images_bootstrap_5g_guide/image%208.png)
- Takes 10-15mins bootstrapping the management cluster.
- Will create 1 Control node and 2 worker nodes

  ![image.png](../images/images_bootstrap_5g_guide/image%209.png)

#### Step 4

- Accessing the Management Cluster Via SSH

  - Management cluster nodes

  ![image.png](../images/images_bootstrap_5g_guide/image%2010.png)
- SSH into the control node to access nephio components and create workload clusters

#### Step 5

- Check all the pods and repositories running in the management cluster

  ![image.png](../images/images_bootstrap_5g_guide/image%2011.png)
- Stock  and management repositories

  ![image.png](../images/images_bootstrap_5g_guide/image%2012.png)

#### Step 6

- Accessing the Management Cluster Via webui

  ![image.png](../images/images_bootstrap_5g_guide/image%2013.png)

  ![image.png](../images/images_bootstrap_5g_guide/image%2014.png)

#### Step 7

- Accessing the Nephio Via webui

1. Shows all registered repository, containing Infra resources for deployment
2. Contains core packages for respective clusters in deployment
3. All registered repositories from upstream provider

  ![image.png](../images/images_bootstrap_5g_guide/image%2015.png)

#### Step 8

- Creating workload clusters (core, regional and edge)

1. Clone the git repo with packagevariants already created to create 3 clusters with one command and apply resources

  ![image.png](../images/images_bootstrap_5g_guide/image%2016.png)

1. 3 drafts revisions for each cluster created

  ![image.png](../images/images_bootstrap_5g_guide/image%2017.png)

1. If multi-cluster connectivity is a priority, delete the flannel PackageVariant from each clusters’ revision

  ![image.png](../images/images_bootstrap_5g_guide/image%2018.png)

#### Step 9

- Creating workload clusters (core, regional and edge)

  ![image.png](../images/images_bootstrap_5g_guide/image%2019.png)

  ![image.png](../images/images_bootstrap_5g_guide/image%2020.png)

  Some infra and core packages in drafts in both mgmt and mgmt-staging respectively requires manual editing to add AWS variables and add git server URL and port

  ![image.png](../images/images_bootstrap_5g_guide/image%2021.png)

#### Step 10

- After 2-5mins we have the instances created, up and running and each cluster will have its own git repo.

  ![image.png](../images/images_bootstrap_5g_guide/image%2022.png)
- Next, we have to get kubeconfig and install Cilium CNI otherwise if cluster interconnectivity is not required, no need to install a CNI as it will automatically be configured with Nephio through ArgoCD

  ![image.png](../images/images_bootstrap_5g_guide/image%2023.png)

  ![image.png](../images/images_bootstrap_5g_guide/image%2024.png)

#### Step 11

- Installing cilium CNI on the workload clusters (core, regional and edge)

  ![image.png](../images/images_bootstrap_5g_guide/image%2025.png)
- install Cilium CNI otherwise if cluster interconnectivity is not required, no need to install a CNI as it will automatically be configured with Nephio through ArgoCD

  ![image.png](../images/images_bootstrap_5g_guide/image%2026.png)
- After installing Cilium, we have to interconnect the clusters using the ciliumctl

#### Step 12

- Installing cilium CNI on the workload clusters (core, regional and edge)
- Edge and core clusters interconnected:

  ![image.png](../images/images_bootstrap_5g_guide/image%2027.png)
- install Cilium CNI otherwise if cluster interconnectivity is not required, no need to install a CNI as it will automatically be configured with Nephio through ArgoCD
- After installing Cilium, we have to interconnect the clusters using the ciliumctl

#### Step 13

- Installing cilium CNI on the workload clusters (core, regional and edge)

  ![image.png](../images/images_bootstrap_5g_guide/image%2028.png)

  Regional, Edge and core clusters interconnected and passed the cilium interconnectivity test:

  ![image.png](../images/images_bootstrap_5g_guide/image%2029.png)
- install Cilium CNI otherwise if cluster interconnectivity is not required, no need to install a CNI as it will automatically be configured with Nephio through ArgoCD
- After installing Cilium, we have to interconnect the clusters using the ciliumctl

#### Step 14

- Iperf Test – testing pod-pod communication between clusters
- Iperf test conducted between edge and core cluster run successfully

  ![image.png](../images/images_bootstrap_5g_guide/image%2030.png)
