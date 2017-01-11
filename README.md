# What is it?

Google deployment manager allows you to define your google cloud infrastructure
as yaml configuration.

It's similar to [terraform](https://www.terraform.io/) and 
[aws cloudformation.](https://aws.amazon.com/cloudformation/)

# Ok, let's get going!

Not so fast, zergling. Make sure you've set up a google cloud project 
[(https://cloud.google.com/compute/docs/projects)](https://cloud.google.com/compute/docs/projects) 
and the gcloud command line tool to interact with it 
[(https://cloud.google.com/sdk/gcloud/.)](https://cloud.google.com/sdk/gcloud/) 

# And now?

Now, we get to play. At it's very simplest, deployment manager (from herein
referred to as DM,) takes one config (yaml) file as input, and creates
infrastructure based on the content of the config file. For example - vm.yml can
be used to create a new vm:

```yaml
# NB: Alter MYPROJECT to the name of your project
resources:
- name: my-vm
  type: compute.v1.instance
  properties:
    zone: us-central1-a
    machineType: https://www.googleapis.com/compute/v1/projects/MYPROJECT/zones/us-central1-f/machineTypes/f1-micro
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/family/debian-8
    networkInterfaces:
    - network: https://www.googleapis.com/compute/v1/projects/MYPROJECT/global/networks/default
      accessConfigs:
      - name: External NAT
        type: ONE_TO_ONE_NAT
```

Resources will have a name, and a type. Depending on what their type is, they
may require additional properties to be configured too. You can learn more about
what the different types are in the [documentation.](https://cloud.google.com/deployment-manager/docs/configuration/create-configuration-file#supported_resource_types_and_properties)

# Yes, but nothing got created yet, right?

Right - a perfectly astute if impatient remark. To create a deployment:

```bash
gcloud deployment-manager deployments create DEPLOYMENT_NAME --config vm.yml
```
