---
title: A YANG Data Model for Open Shortest Path First (OSPF) Topology
abbrev: OSPF Topology YANG
docname: draft-ogondio-nmop-ospf-topology-latest

stand_alone: true
ipr: trust200902
area: "Operations and Management"
wg: nmop
category: std
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"

consensus: true
v: 3

author:
  -
    fullname: Oscar González de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    fullname: Samier Barguil Giraldo
    org: Nokia
    email: samier.barguil_giraldo@nokia.com
  -
    fullname: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com

#contributor:
#  -


--- abstract

This document defines a YANG data model for representing an abstracted view of a network topology that contains Open Shortest Path First (OSPF) information. This document augments the 'ietf-network' data model by adding OSPF concepts and explains how the data model can be used to represent the OSPF topology.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture (NMDA).


--- middle

# Introduction

Network operators perform the capacity planning for their networks and run regular what-if scenarios analysis based on representations of the real network. Those what-if analysis and capacity planning processes require, among other information, a topological view (domains, nodes, links, network interconnection) of the deployed network.

This document defines a YANG data model representing an abstracted view of a network topology containing Open Shortest Path First (OSPF). It covers the topology of IP/MPLS networks running OSPF as Interior Gateway Protocol (IGP) protocol. The proposed YANG model augments the "A YANG Data Model for Network Topologies" {{!RFC8345}} and "A YANG Data Model for Layer 3 Topologies" {{!RFC8346}} by adding OSPF concepts. It is worth to highlight that the Yang model can also be used together with {{!RFC8795}} and {{?I-D.draft-ietf-teas-yang-l3-te-topo}} when Traffic engineering characteristics are required in the topological view.

This YANG data model can be used to export the OSPF related topology directly from a network controller to Operation Support System (OSS) tools or to a higher level controller.

Note that the YANG model is in this document strictly adheres to the concepts (and the YANG module) in "A YANG Data Model for Network Topologies" {{!RFC8345}} and "A YANG Data Model for Layer 3 Topologies" {{!RFC8346}}. While investigating the OSFP topology, some limitations have discovered in {{!RFC8345}}, regarding how the digital map can be represented. Those limitations (and potential improvements) are covered in {{?I-D.draft-havel-nmop-digital-map}}.

This document explains the scope and purpose of the OSPF topology model and how the topology and service models fit together.
The YANG data model defined in this document conforms to the Network Management Datastore Architecture {{!RFC8342}}.

## Terminology and Notations

This document assumes that the reader is familiar with OSPF and the contents of {{!RFC8345}}. The document uses terms from those documents.

The terminology for describing YANG data models is found in {{!RFC7950}}, {{!RFC8795}} and {{!RFC8346}}.

The term Digital Twin, Digital Map, Digital Map Modelling, Digital Map Model, Digital Map Data, and Topology are specified in {{?I-D.draft-havel-nmop-digital-map}}.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Tree Diagram

Authors include a simplified graphical representation of the data model specified in  {{ietf-l3-ospf-topology-tree}} of this document.
The meaning of the symbols in these diagrams is defined in {{!RFC8340}}.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects are prefixed using the standard prefix associated with the corresponding YANG imported modules, as shown in the following table.

| Prefix | Yang Module            | Reference    |
| ------ | ---------------------- | ------------ |
| ospfnt | ietf-l3-ospf-topology  | RFCXXX       |
| yang   | ietf-yang-types        | {{!RFC6991}} |
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please remove this note.

# Use Cases

Use cases for this document are the same than explained in {{?I-D.draft-ogondio-nmop-isis-topology}}. Here are included for completeness and discussion. Future versions may consider removing them.

This information is required in the IP/MPLS planning process to properly assess the required network resources to meet the traffic demands in normal and failure scenarios. Network operators perform the capacity planning for their networks and run regular what-if scenarios analysis based on representations of the real network. Those what-if analysis and capacity planning processes require, among other information, a topological view (domains, nodes, links, network interconnection) of the deployed network.

The standardization of an abstracted view of the OSPF topology model as NorthBound Interface (NBI) of Software Defined Networking (SDN) controllers allows the unified query of the OSFP topology in order to inject this information into third party tools covering specialized cases.

The OSFP topological model should export enough OSFP information to permit these tools to simulate the IP routing. By mapping the traffic demand, ideally at the IP flow level, to the topology, we can simulate the traffic growth, evaluating this way its effect on the routing and quality of service. That is, simulating how IP-level traffic demands would be forwarded, after OSPF convergence is reached, and from there estimating, using appropriate mathematical models, related KPIs like the occupation in the links or end-to-end latencies.

In summary, the network-wide view of the OSFP topology enables multiple use cases:

* Network design: verifying that the actual deployed OSFP network conforms to the planned design.
+ Capacity planning. Dimensioning or redesign of the IP infrastructure to satisfy target KPI metrics under existing or forecasted traffic demands.
+ What-if analysis. Estimation of the network KPIs in modified network situations. For instance, failure situations, traffic anomaly situations, addition or deletion of new adjacencies, IGP weight reconfigurations, etc.
- Failure analysis. Systematic and massive test of the network under multiple simulated failure situations, evaluating the network fault tolerance properties, and using mathematical models to derive statistical network availability metrics.

## Relationship with the OSPF YANG Model

{{!RFC9129}} specifies a YANG data model that can be used to configure and manage the OSPF protocol on network elements. This data model covers the configuration of an OSPF routing protocol instance, as well as the retrieval of OSPF operational states.
{{!RFC9129}} is still expected to be used for individual network elements configuration and monitoring. On the other hand, the proposed YANG model in this document covers the abstracted view of the entire network topology containing OSPF. As such, this model is aimed at being available via the NBI of an SDN controller.

## Relationship with Digital Map

As described in {{?I-D.draft-havel-nmop-digital-map}}, the Digital Map provides the core multi-layer topology model and data for the digital twin and connects them to the other digital twin models and data.

The Digital Map Modelling defines the core topological entities, their role in the network, core properties, and relationships both inside each layer and between the layers.

The Digital Map Model is a basic topological model that is linked to other functional parts of the digital twin and connects them all: configuration, maintenance, assurance (KPIs, status, health, symptoms), Traffic Engineering (TE), different behaviors and actions,  simulation, emulation, mathematical abstractions, AI algorithms, etc.

As such the IGP topology of the Digital Map (in this case, OSPF) is just one of the layers of the Digital Map, for specific user (the network operator in charge of the IGP) for specific IGP use cases as described before.

# YANG Data Model for OSPF Topology

The abstract (base) network data model is defined in the "ietf-network" module of {{!RFC8345}}. The OSPF-topology builds on the network data model defined in the "ietf-network" module {{!RFC8345}}, augmenting the nodes with OSPF information, which anchor the links and are contained in nodes.

There is a set of parameters and augmentations that are included at the node level. Each parameter and description are detailed following:

* Network-types: Its presence identifies the OSPF topology type. Thus, the network type MUST be ospf-topology.
+ OSPF timer attributes: Identifies the node timer attributes configured in the network element. They are wait timer, rapid delay, slow delay, and the timer type (linear or exponential back-off).
- OSPF status: contains the neighbours' information.

The following figure is based on the Figure 1 from {{!RFC8346}}, where the example-ospf-topology is replaced with ietf-l3-ospf-topology and where
arrows show how the modules augment each other.

{: #ietf-l3-ospf-topology-module-structure}
~~~~
                      +-----------------------------+
                      |  +-----------------------+  |
                      |  |      ietf-network     |  |
                      |  +----------^------------+  |
                      |             |               |
                      |  +-----------------------+  |
                      |  | ietf-network-topology |  |
                      |  +----------+------------+  |
                      +-------------^---------------+
                                    |
                                    |
                       +------------^-------------+
                       | ietf-l3-unicast-topology |
                       +------------^-------------+
                                    |
                                    |
                        +-----------^-----------+
                        | ietf-l3-ospf-topology |
                        +-----------------------+
~~~~
{: #fig-ietf-l3-ospf-topology-module-structure title="OSPF Topology module structure"}

A second set of parameters, along with augmentations, is included at the link and termination point level. Each parameter is listed as follows:

* Interface-type
+ Area ID
+ Metric
- Passive mode

{: #ietf-l3-ospf-topology-tree}

# RFC8345 Limitations for the OSPF Modeling

There are some limitations in the {{!RFC8345}} that are explained in more detail in {{!I-D.draft-havel-nmop-digital-map}}.
The current version of the ietf-l3-ospf-topology module is based on the current version of {{!RFC8345}}.

# OSPF Topology Tree Diagram

{{fig-ietf-l3-ospf-topology-tree}} below shows the tree diagram of the YANG data model defined in module ietf-l3-ospf-topology.yang ({{ietf-l3-ospf-topology-yang}}).

~~~~
module: ietf-l3-ospf-topology
  augment /nw:networks/nw:network/nw:network-types:
    +--rw ospfv2-topology!
  augment /nw:networks/nw:network/nw:node/
    l3t:l3-node-attributes:
    +--rw ospf-timer-attributes
       +--rw wait-timer?    uint32
       +--rw rapid-delay?   uint32
       +--rw slow-delay?    uint32
       +--rw timer-type?    enumeration
  augment /nw:networks/nw:network/nt:link/
    l3t:l3-link-attributes:
    +--rw ospfv2-termination-point-attributes
       +--rw interface-type?   identityref
       +--rw area-id?          area-id-type
       +--rw metric?           uint64
       +--rw is-passive?       boolean
  augment /nw:networks/nw:network/nw:node/nt:termination-point/
    l3t:l3-termination-point-attributes:
    +--rw ospfv2-termination-point-attributes
       +--rw interface-type?   identityref
       +--rw area-id?          area-id-type
       +--rw metric?           uint64
       +--rw is-passive?       boolean
~~~~
{: #fig-ietf-l3-ospf-topology-tree title="OSPF Topology tree diagram"}

{: #ietf-l3-ospf-topology-yang}

# YANG Model for OSPF topology

Following the YANG model is presented.

~~~~
<CODE BEGINS> file "ietf-l3-ospf-topology@2024-06-12.yang"

module ietf-l3-ospf-topology {
  yang-version 1.1;
  namespace
    "urn:ietf:params:xml:ns:yang:ietf-l3-ospf-topology";
  prefix "ospfnt";
  import ietf-yang-types {
          prefix "yang";
      }
  import ietf-network {
    prefix "nw";
  }
  import ietf-network-topology {
    prefix "nt";
  }
  import ietf-l3-unicast-topology {
    prefix "l3t";
  }

  organization
    "IETF NMOP (Network Management Operations) Working Group";
  contact
    "WG Web:  <https://datatracker.ietf.org/wg/opsawg/>
    WG List:  <mailto:opsawg@ietf.org>

    Editor:   Oscar Gonzalez de Dios
              <mailto:oscar.gonzalezdedios@telefonica.com>
    Editor:   Samier Barguil
              <mailto:samier.barguilgiraldo.ext@telefonica.com>
    Editor:   Victor Lopez
              <mailto:victor.lopez@nokia.com>";
  description
    "This module defines a model for Layer 3 OSPF
     topologies.

     Copyright (c) 2024 IETF Trust and the persons identified as
     authors of the code.  All rights reserved.

     Redistribution and use in source and binary forms, with or
     without modification, is permitted pursuant to, and subject to
     the license terms contained in, the Revised BSD License set
     forth in Section 4.c of the IETF Trust's Legal Provisions
     Relating to IETF Documents
     (https://trustee.ietf.org/license-info).

     This version of this YANG module is part of RFC XXXX
     (https://www.rfc-editor.org/info/rfcXXXX); see the RFC itself
     for full legal notices.";

  revision 2022-03-07 {
    description
      "Initial version";
    reference
      "RFC XXXX: A YANG Data Model for Open Shortest Path First
       (OSPF) Topology"; }

  typedef area-id-type {
    type yang:dotted-quad;
    description
      "An identifier for the OSPFv2 area.";
    reference
         "RFC 2328: OSPF Version 2";
  }

  identity inf-type {
    description
      "Identity for the OSPF interface type.";
    reference
         "RFC 2328: OSPF Version 2";
  }

  identity nbma {
    base inf-type;
    description
      "Identity for the NBMA interface.";
    reference
         "RFC 2328: OSPF Version 2";
  }

  identity p2mp {
    base inf-type;
    description
      "Identity for the p2mp interface.";
    reference
         "RFC 2328: OSPF Version 2";
  }
  identity p2mp-over-lan {
    base inf-type;
    description
      "Identity for the p2mp-over-lan interface.";
    reference
         "RFC 2328: OSPF Version 2";
  }

  identity p2p {
    base inf-type;
    description
      "Identity for the p2p interface.";
    reference
         "RFC 2328: OSPF Version 2";
  }

  grouping ospfv2-topology-type {
    description "Identifies the topology type to be OSPF v2.";
    container ospfv2-topology {
      presence "indicates OSPF v2 topology";
      description
        "The presence of the container node indicates OSPF v2
        topology";
    }
  }

  grouping ospfv2-node-attributes {
    description "OSPF v2 node scope attributes";
    container ospf-timer-attributes {
      description
        "Contains OSPFv2 node timer attributes";
      leaf wait-timer {
        type uint32;
        units msec;
        description
          "The amount of time to wait without detecting SPF
          trigger events before going back to the rapid delay.";
        reference
         "RFC 8541: SPF Impact on IGP Micro-loops";
      }
      leaf rapid-delay {
        type uint32;
        units msec;
        description
          "The amount of time to wait before running SPF after
          the initial SPF trigger event.";
        reference
         "RFC 8541: SPF Impact on IGP Micro-loops";
      }
      leaf slow-delay {
        type uint32;
        units msec;
        description
          "The amount of time to wait before running an SPF.";
        reference
         "RFC 8541: SPF Impact on IGP Micro-loops";
      }
      leaf timer-type {
        type enumeration {
          enum LINEAR_BACKOFF {
            description
              "The link state routing protocol uses linear
               back-off.";
          }
          enum EXPONENTIAL_BACKOFF {
            description
              "The link state routing protocol uses exponential
               back-off.";
          }
        }
        description
          "The timer mode that is utilised by the SPF algorithm.";
        reference
         "RFC 8541: SPF Impact on IGP Micro-loops";
      }
    }
  }

  grouping ospfv2-termination-point-attributes {
    description "OSPF termination point scope attributes";
    container ospfv2-termination-point-attributes {
      description
        "Indicates the termination point from the
              which the OSPF is configured. A termination
              point can be a physical port, an interface, etc.";
      leaf interface-type {
        type identityref {
          base inf-type ;
        }
        description
          "OSPF interface type.";
        reference
          "RFC 2328: OSPF Version 2";
      }
      leaf area-id {
        type area-id-type;
        description
          "An identifier for the OSPFv2 area.";
        reference
          "RFC 2328: OSPF Version 2";
      }
      leaf metric {
        type uint64;
        description
          "OSFP Protocol metric";
        reference
          "RFC 2328: OSPF Version 2";
      }
      leaf is-passive{
        type boolean;
        description
          "Interface passive mode";
        reference
          "RFC 2328: OSPF Version 2";
      }
    }
  }

  augment "/nw:networks/nw:network/nw:network-types" {
    description
      "Introduces new network type for L3 Unicast topology";
    uses ospfv2-topology-type;
  }

  augment "/nw:networks/nw:network/nw:node/"
    +"l3t:l3-node-attributes" {
    when
    "/nw:networks/nw:network/nw:network-types/"
      +"ospfnt:ospfv2-topology" {
      description
        "Augmentation parameters apply only for networks with
        OSPF topology";
    }
    description
        "OSPF node-level attributes ";
    uses ospfv2-node-attributes;
  }

  augment "/nw:networks/nw:network/"
      + "nt:link/l3t:l3-link-attributes" {
    when "/nw:networks/nw:network/nw:network-types/"
      +"ospfnt:ospfv2-topology" {
      description
        "Augmentation parameters apply only for networks with
        OSFP topology";
    }
    description "Augments topology link configuration";
    uses ospfv2-termination-point-attributes;
  }

  augment "/nw:networks/nw:network/nw:node/"
      +"nt:termination-point/l3t:l3-termination-point-attributes" {
    when "/nw:networks/nw:network/nw:network-types/"
      +"ospfnt:ospfv2-topology" {
      description
        "Augmentation parameters apply only for networks with
        OSFP topology";
    }
    description "Augments topology termination point configuration";
    uses ospfv2-termination-point-attributes;
  }
}

<CODE ENDS>
~~~~
{: #fig-ietf-ospf-topolopy-yang title="OSPF Topology YANG module"}

# Security Considerations


  The YANG module specified in this document defines a schema for data that is designed to be accessed via network management protocols such as NETCONF {!RFC6241}} or RESTCONF {{!RFC8040}}. The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{!RFC8446}}.

  The Network Configuration Access Control Model (NACM) {{!RFC8341}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.

 There are a number of data nodes defined in this YANG module that are writable/creatable/deletable (i.e., config true, which is the default). These data nodes may be considered sensitive or vulnerable in some network environments. Write operations (e.g., edit-config) to these data nodes without proper protection can have a negative effect on network operations.

# IANA Considerations

  This document registers the following namespace URIs in the IETF XML registry {{!RFC3688}}:

    --------------------------------------------------------------------
    URI: urn:ietf:params:xml:ns:yang:ietf-l3-ospf-topology
    Registrant Contact: The IESG.
    XML: N/A, the requested URI is an XML namespace.
    --------------------------------------------------------------------

   This document registers the following YANG module in the YANG Module Names registry {{!RFC6020}}:

    --------------------------------------------------------------------
    name:         ietf-l3-ospf-topology
    namespace:    urn:ietf:params:xml:ns:yang:ietf-l3-ospf-topology
    maintained by IANA: N
    prefix:       ietf-l3-ospf-topology
    reference:    RFC XXXX
    --------------------------------------------------------------------

# Implementation Status

This section will be used to track the status of the implementations of the model. It is aimed at being removed if the document becomes RFC.

--- back

{: numbered="false"}

# Acknowledgments
{:numbered="false"}

This work is partially supported by the European Commission under Horizon 2020 Secured autonomic traffic management for a Tera of SDN flows (Teraflow) project (grant agreement number 101015857).
