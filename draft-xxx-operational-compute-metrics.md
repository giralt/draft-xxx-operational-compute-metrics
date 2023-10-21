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

informative:


--- abstract

TODO Abstract


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Motivations

TODO


# Problem Space and Needs

Visibility and exposure to the application of both (1) network and (2) compute
resources is critical to
enable the proper functioning of the new class of services
arising at the edge (distributed AI, driverless vehicles,
AR/VR, etc.). To understand the problem space and the capabilities
that are lacking in today's protocol interfaces necessary to
properly enable these new services, we focus on the simple life cycle of
service deployment and selection ({{lifecycle}}).


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

Compute nodes are deployed at the edge (e.g., co-located
in a 5G base station) to provide computing services that are
close to users with the goal to (1) reduce latency, (2) increase
communication bandwidth, (3) enable privacy/personalization
(e.g., federated AI learning), and (4) reduce cloud costs and
energy. Services are deployed on the communication and compute
infrastructure through a two-phase life cycle that involves first a
service *deployment stage* and then a *service selection* stage
({{lifecycle}}).

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

**Service selection.**



#  Guiding Principles

TODO



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
