---
title: "Joint Exposure of Network and Compute Information for Infrastructure-Aware Service Deployment"
abbrev: "TODO - Abbreviation"
category: info

docname: draft-rcr-opsawg-operational-compute-metrics-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: AREA
# workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "giralt/draft-rcr-opsawg-operational-compute-metrics"
  latest: "https://giralt.github.io/draft-rcr-opsawg-operational-compute-metrics/draft-rcr-opsawg-operational-compute-metrics.html"

author:
 -
    fullname: "S. Randriamasy"
    organization: Nokia Bell Labs
    email: "sabine.randriamasy@nokia-bell-labs.com"

 -
    fullname: "L. M. Contreras"
    organization: Telefonica
    email: "luismiguel.contrerasmurillo@telefonica.com"

 -
    fullname: "Jordi Ros-Giralt"
    organization: Qualcomm Europe, Inc.
    email: "jros@qti.qualcomm.com"

normative:
  RFC7285:
  I-D.ietf-alto-performance-metrics:
  I-D.du-cats-computing-modeling-description:

informative:
   NFV-TST:
              title: "ETSI GS NFV-TST 008 V3.3.1, NFVI Compute and Network Metrics Specification"
              date: 2020-06-01
              target: https://www.etsi.org/deliver/etsi_gs/NFV-TST/001_099/008/03.03.01_60/gs_NFV-TST008v030301p.pdf

   NFV-INF:
              title: "ETSI GS NFV-INF 010, v1.1.1, Service Quality Metrics"
              date: 2014-12-01
              target: https://www.etsi.org/deliver/etsi_gs/NFV-INF/001_099/010/01.01.01_60/gs_NFV-INF010v010101p.pdf

--- abstract

Service providers are starting to deploy computing capabilities
across the network for hosting applications such as AR/VR, vehicle
networks, IoT, and AI training, among others. In these distributed
computing environments, information about computing and communication
resources is necessary to determine both the proper deployment location of
each application and the best server location on which to run it.
This information is used by numerous different implementations with different
interpretations. This document proposes an initial approach towards a
common understanding and exposure scheme for metrics reflecting compute
capabilities.

--- middle

# Introduction

Operators are starting to deploy distributed computing environments
in different parts of the network with the objective of addressing
different service needs including latency, bandwidth, processing
capabilities, storage, etc.
This translates in the emergence of a
number of data centers (both in the cloud and at the edge) of
different sizes (e.g., large, medium, small) characterized by
distinct dimension of CPUs, memory, and storage capabilities, as well
as bandwidth capacity for forwarding the traffic generated in and out
of the corresponding data center.

The proliferation of the edge computing paradigm further increases
the potential footprint and heterogeneity of the environments where a
function or application can be deployed, resulting in different
unitary cost per CPU, memory, and storage. This increases the
complexity of deciding the location where a given function or
application should be best deployed or executed.
This decision should be jointly
influenced on the one hand by the available resources in a given computing
environment, and on the other hand by the capabilities of the network path connecting
the traffic source with the destination.

Network and compute aware function placement and selection has become of utmost importance in the last decade. The availability of such information is taken for granted by the numerous service providers and bodies that are specifying them.
However, deployments may reach out to data centers running different implementations with different understandings and representations of compute capabilities and smooth operation is a challenge. While standardization efforts on network capabilities representation and exposure are well-advanced, similar efforts on compute capabilitites are in their infancy.

This document proposes an initial approach towards  a common understanding
and exposure scheme for metrics reflecting compute capabilities.
It aims at leveraging on existing work in the IETF on compute metrics definitions to build synergies.
It also aims at reaching out to working or research groups in the IETF that would consume such information and have particular requirements.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

<!-- # Motivations

TODO TEST
ANOTHE CHANGE + COMMENTS
CHANGE 2 -->


# Problem Space and Needs

Visibility and exposure of both (1) network and (2) compute
resources to the application is critical to
enable the proper functioning of the new class of services
arising at the edge (e.g., distributed AI, driverless vehicles,
AR/VR, etc.). To understand the problem space and the capabilities
that are lacking in today's protocol interfaces needed to enable
these new services, we focus on the life cycle of
a service.

At the edge, compute nodes
are deployed near communication nodes (e.g., co-located
in a 5G base station) to provide computing services that are
close to users with the goal to (1) reduce latency, (2) increase
communication bandwidth, (3) enable privacy/personalization
(e.g., federated AI learning), and (4) reduce cloud costs and
energy. Services are deployed on the communication and compute
infrastructure through a two-phase life cycle that involves first a
service *deployment stage* and then a *service selection* stage
({{lifecycle}}).

     +-------------+      +--------------+      +-------------+
     |             |      |              |      |             |
     |  New        +------>  Service     +------>  Service    |
     |  Service    |      |  Deployment  |      |  Selection  |
     |             |      |              |      |             |
     +-------------+      +--------------+      +-------------+
{: #lifecycle title="Service life cycle." }


<!--
In this Section, we introduce the life cycle of a
service as a simple framework to understand the capabilities
that are lacking in today's protocol interfaces and that are necessary for
these new services. -->



**Service deployment.** This phase is carried out by the service
provider, and consists in the deployment of a new service
(e.g., a distributed AI training/inference, an XR/AR service, etc.)
on the communication and compute infrastructure. The service
provider needs to properly size the amount of communication and compute
resources assigned to this new service to meet the expected user
demand. The decision on where the service is deployed and how
many resources are requested from the infrastructure depends on
the levels of QoE that the provider wants to guarantee to the
user base. To make a proper deployment decision, the provider
must have visibility on the resources available from
the infrastructure, including communication resources (e.g., latency and
bandwidth) and compute (e.g., CPU, GPU, memory, storage). For instance,
to run a Large Language Model (LLM) with 175 billion parameters, a total
aggregated memory of 400GB and 8 GPUs are needed. The service provider needs
an interface to query the infrastructure, extract the available compute
and communication resources, and decide which subset of resources are
needed to run the service.

**Service selection.** This phase is initiated by the user, through
a client application that connects to the deployed service. There
are two main decisions that must be performed in the service selection
stage: compute node selection and path selection. In the compute node selection
step, as the service is generally replicated in
N locations (e.g., by leveraging a microservices architecture),
the application must decide which of the service replicas
it connects to. Similar to the service deployment stage, this
decision requires knowledge about communication and compute
resources available in each replica. On the other hand, in the path selection decision,
the application must decide which path it chooses to connect to the service.
This decision depends on the communication properties (e.g., bandwidth and latency)
of the available paths. Similar to the service deployment case,
the service provider needs an interface to query the infrastructure and extract the available compute
and communication resources, with the goal to make informed node and path selection decisions.
It is also important to note that, ideally, the node and path selection
decisions should be jointly optimized, since in general the best end-to-end performance
is achieved by jointly taking into account both decisions. In some cases, however,
such decisions may be owned by different players. For instance, in some network
environments, the path selection may be decided by the network operator,
wheres the node selection may be decided by the application. Even in these cases,
it is crucial to have a proper interface (for both the network operator and the service
provider) to query the available compute and communication resources from the system.

{{prob_space}} summarizes the problem space, the information that needs to be exposed, and the stakeholders that need this information.

| Action to take | Information needed | Who needs it |
|-----------:|:----------------------:|:----------------------|
| Service placement | Compute and communication | Service provider |
| Service selection/node selection | Compute | Network/service provider and/or application |
| Service selection/path selection | Communication | Network/service and/or application |
{: #prob_space title="Problem space, needs, and stakeholders." }

## Scenarios of production and consumption of compute-related information
It is important to understand the scenarios of production and consumption of compute-related information in combination with network status. The combination of both network and compute magnify the possibilities of optimization on the usage of the resources on both environments, as well as the improvements that can be observed by customers at service level because of such optimization.

### Producers of compute-related information

The information relative to compute (i.e., processing capabilities, memory and storage capacity) can be structured in two ways. On one hand, the information corresponding to the raw compute resources, and on the other hand, the information of resources allocated or in use by a specific application or service function.

The former is typically provided by the management systems enabling the virtualization of the physical resources for a later assignment to processes running on top. Cloud Managers or Virtual Infrastructure Managers are the entities which manage those resources. These management systems offer APIs from where to retrieve the available resources in a compute facility. Thus, it can be expected the consumption of that APIs for obtaining such information.  Once retrieved the raw resources from different compute facilities, it could be possible to generate topological network views of them, as being proposed in {{?I-D.llc-teas-dc-aware-topo-model}}.

Regarding the resources allocated or in use by a specific application or service function, two situations apply. The total allocation and the allocation per service or application. In the first case, the information can be supplied by the virtualization management systems described before. For the specific allocation per service, it can be expected that the specific management systems of the service or application is capable to provide the resources being used at running time typically as part of the allocated ones. Also in this last scenario it is reasonable to expect the availability of APIs offering such information, even though it can be particular per service or application. 

### Consumers of compute-related information

The consumption of compute-related information is relative to the different phases of the service lifecycle. This means that such information can be consumed in different points of time and for different purposes. 

The expected consumers can be both external or internal to the network. As external consumers it is possible to consider external application management systems requiring resource availability information (among others) for service function placement decision, or workload migration in the case of consuming raw resources, or requiring information on the usage of resources for service assurance or service scaling. 

As internal consumer it is possible to consider network management entities requiring view on the level of resource usage for traffic steering (as the Path Selector in {{?I-D. ldbc-cats-framework }}), load balance, analytics, etc.

#  Guiding Principles

The driving principles for designing an interface to jointly extract network and compute information are as follows:

P1. Leverage metrics across working groups to avoid reinventing the wheel. For instance:

- RFC 9439 {{I-D.ietf-alto-performance-metrics}} leverages IPPM metrics from RFC 7679.
- Section 5.2 of {{I-D.du-cats-computing-modeling-description}} considers delay as a good metric, since it is easy to use in both compute and communication domains. RFC 9439 also defines delay as part of the performance metrics.
- Section 6 of {{I-D.du-cats-computing-modeling-description}} proposes to represent the network structure as graphs, which is similar to the ALTO map services in {{RFC7285}}.

P2. Aim for simplicity, while ensuring the combined efforts
don’t leave technical gaps in supporting the full life cycle
of service deployment and selection. For instance, the CATS working
group is covering path selection from a network standpoint, while
ALTO (e.g., {{RFC7285}}) covers exposing of network information
to the service provider and the client application. However,
there is currently no effort being pursued to expose compute information
to the service provider and the client application for
service placement or selection.

# Related Work

Some existing work has explored compute-related metrics. They can be categorized as follows:

* References providing raw compute infrastructure metrics: {{?I-D.contreras-alto-service-edge}} includes references to cloud management solutions (i.e., OpenStack, Kubernetes, etc) which administer the virtualization infrastructure, providing information about raw compute infrastructure metrics. Furthermore, {{NFV-TST}} describes processor, memory and network interface usage metrics.
* References providing compute virtualization metrics: {{?RFC7666}} provides several metrics as part of the Management Information Base (MIB) definition for managing virtual machines controlled by a hypervisor. The objects there defined make reference to the resources consumed by a particluar virtual machine serving as host for services or applications. Moreover, {{NFV-INF}} provides metrics associated to virtualized network functions.
* References providing service metrics including compute-related information: {{?I-D.dunbar-cats-edge-service-metrics}} proposes metrics associated to services running in compute infrastructures. Some of these metrics do not depend on the infrastructure behavior itself but from where such compute infrastructure is topologically located.

# GAP Analysis

From this related work it is evident that compute-related metrics can serve several purposes, ranging from service instance instantiation to service instance behavior, and then to service instance selection. Some of the metrics could refer to the same object (e.g., CPU) but with a particular usage and scope.

In contrast, the network metrics are more uniform and straightforward. It is then necessary to consistently define a set of metrics that could assist to the operation in the different concerns identified so far, so that networks and systems could have a common understanding of the perceived compute performance. When combined with network metrics, the combined network plus compute performance behavior will assist informed decisions particular to each of the operational concerns related to the different parts of a service life cycle.



# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
