BOSH release to run cassandra
=======================

Background
----------

### What is cassandra?

cassandra aggregates events from your servers and applications with a powerful stream processing language. Send an email for every exception in your app. Track the latency distribution of your web app. See the top processes on any host, by memory and CPU. Combine statistics from every Riak node in your cluster and forward to Graphite. Track user activity from second to second.

Usage
-----

To use this bosh release, first upload it to your bosh:

```
bosh upload release https://github.com/hybris/cassandra-boshrelease
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

```yaml
---
networks:
  - name: cassandra1
    type: dynamic
    cloud_properties:
      security_groups:
        - cassandra
```

Where `- cassandra` means you wish to use an existing security group called `cassandra`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```
