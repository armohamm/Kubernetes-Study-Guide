# Anatomy of an Kubernetes object config file

There's 2 main approaches to create/update/delete Kubernetes objects. They are the [imperative and declarative approaches](https://kubernetes.io/docs/concepts/overview/object-management-kubectl/overview/). 

Broadly speaking, the imperative approach involves creating objects solely via the kubectl command line. Whereas the declaritive approach involves writing yaml descriptors and then use the kubectl 'apply' subcommand to create them. It's recommended to use the declaritive approach 

Notice, that all these config files have the following general yaml structure:

```yaml
apiVersion: xxx
kind: xxxxx
metadata:
  {blah blah blah}
spec:
  {blah blah blah}
```

The apiVersion, kind, metadata, and spec, are the [required fields](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/#required-fields), for all kubernetes object files.

**kind:** What type object that you want to make.

**apiVersion:** The kubernetes api is rapidly evolving so the api is broken down into various parts. Your version choice depends on what 'kind' of object you want to define.  For example, if the kind is 'Pod' then this field needs to be set to 'v1'.

You need to take a look at the [Kubernetes API Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.14/) to work out what to set this for your chosen object type (kind). This reference doc is really useful have displays example yaml content for your chosen kind. This link is for version v1.14. But in your case you need go to the link, that's specified with the Major and Minor tag of Server Version in:

```bash
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.3", GitCommit:"721bfa751924da8d1680787490c54b9179b1fed0", GitTreeState:"clean", BuildDate:"2019-02-04T04:48:03Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.3", GitCommit:"721bfa751924da8d1680787490c54b9179b1fed0", GitTreeState:"clean", BuildDate:"2019-02-01T20:00:57Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
```

You can also view the api reference data from the cli, using the 'explain' subcommand:

```bash
$ kubectl explain pod
KIND:     Pod
VERSION:  v1

DESCRIPTION:
     Pod is a collection of containers that can run on a host. This resource is
     created by clients and scheduled onto hosts.
...
```

You can get a high-level overview of the entire yaml structure:

```bash
kubectl explain pod --recursive
```


And you can drill down like this:


```bash
$ kubectl explain pod.kind
KIND:     Pod
VERSION:  v1

FIELD:    kind <string>

DESCRIPTION:
     Kind is a string value representing the REST resource this object
     represents. Servers may infer this from the endpoint the client submits
     requests to. Cannot be updated. In CamelCase. More info:
     https://git.k8s.io/community/contributors/devel/api-conventions.md#types-kinds
...
```


**metadata:** Data that helps uniquely identify the object. metadata.name is used to assign a name to the object. metadata.labels is also another really important feature. It not only lets you organise your resources, but it also offers a means to filter your resources, and can be used as a way to refer to a group of resources. 

**spec:** The content of this depends on the kind of object in question. Api specifies what structure+content this section should hold.

## Defining multiple objects in a single config file

In this walkthrough we ended up with 2 config files. However you can store 2 or more objects in a single config file. All you need to do is to copy all the definitions into a single file, and seperate them out using by inserting the yaml-new-document-syntax '---' between them. It's really a preference on whether or not to use this approach.


## Updating objects

Kubernetes is smart enough to identify which objects have been created by a particular config file. It does so by using the configs about the 'kind' and metadata.name info. The yaml descriptor's filename itself doesn't matter, as long as it ends with the .yml/.yaml extension. You can make changes to the yaml files (as long as it isn't changing the 'kind' or 'metadata.name' fields) and just apply them again for the changes to take affect. 


## View entire yaml definition including implicit values. 



## References

[kubernetes api concepts](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)
[kubernetes api reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.13/)