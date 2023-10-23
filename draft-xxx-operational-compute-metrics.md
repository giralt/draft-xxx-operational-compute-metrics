---
title: "Joint Exposure of Network and Compute Information for Infrastructure-Aware Service Deployment"
abbrev: "TODO - Abbreviation"
category: info

docname: draft-xxx-operational-compute-metrics-latest
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
  github: "giralt/draft-xxx-operational-compute-metrics"
  latest: "https://giralt.github.io/draft-xxx-operational-compute-metrics/draft-xxx-operational-compute-metrics.html"

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


--- abstract

Service providers are starting to deploy computing capabilities
 across the network for hosting applications such as AR/VR, vehicle
 networks, IoT, and AI training, among others. In these distributed
 computing environments, information about computing and communication
 resources is necessary to determine both the proper deployment location of
 each application and the best server location on which to run it. 
This information is used by numerous different implementations with different interpretations.
This document proposes an initial approach towards a common understanding 
and exposure scheme for metrics reflecting compute capabilities. 


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Motivations

TODO TEST
ANOTHE CHANGE + COMMENTS


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



#  Guiding Principles

The driving principles for designing an interface to jointly extract network and compute information are as follows:

P1. Leverage metrics across working groups to avoid reinventing the wheel. For instance:

- RFC-to-be 9439 {{I-D.ietf-alto-performance-metrics}} leverages IPPM metrics from RFC 7679.
- Section 5.2 of {{I-D.du-cats-computing-modeling-description}} considers delay as a good metric, since it is easy to use in both compute and communication domains. RFC-to-be 9439 also defines delay as part of the performance metrics.
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

TODO

# GAP Analysis

TODO


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
