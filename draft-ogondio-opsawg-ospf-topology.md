---
title: A YANG Data Model for Open Shortest Path First (OSPF) Topology
abbrev: OSPF Topology YANG
docname: draft-ogondio-opsawg-ospf-topology-latest

stand_alone: true
ipr: trust200902
area: "Operations and Management"
wg: opsawg
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

Topology collection is a critical use case for network operators because the network topology is an abstract representation of the physical nodes, links, and network interconnections. Network planning processes require that network resources are placed to meet traffic demand requirements not just in terms of bandwidth or delay, but also for failure scenarios. Network operators perform the network planning process as an offline process, which obtains information not directly from the network, but from inventory or template information. The main reason for this process was the lack of dynamic and programmatic interfaces that can allow planning tools to obtain such information.

Thanks to the definition of the ietf-network model in {{!RFC8345}} this situation has changed, because network operators can use an API with dynamic topological information. On top of the work in {{!RFC8345}}, {{!RFC8346}} and {{!RFC8944}} extends the generic network and network topology data models with topology attributes that are specific to Layer 3 and Layer 2. However, there is not any model that exposes Open Shortest Path First (OSPF) information. This information is required in the IP/MPLS planning process to properly assess the required network resources to meet the traffic demands in normal and failure scenarios. 

The main objective of this model is to represent most relevant OSPF topology attributes.

This document defines a YANG data model for representing, managing, and controlling the OSPF topology. The data model augments ietf-network module {{!RFC8345}} by adding the OSPF information.

This document explains the scope and purpose of the OSPF topology model and how the topology and service models fit together.

The YANG data model defined in this document conforms to the Network Management Datastore Architecture {{!RFC8342}}.

## Terminology and Notations 

This document assumes that the reader is familiar with OSPF and the contents of {{!RFC8345}}. The document uses terms from those documents.

The terminology for describing YANG data models is found in {{!RFC7950}}, {{!RFC8795}} and {{!RFC8346}}.

## Requirements Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in  {{!RFC2119}}, {{!RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Tree Diagram

Authors include a simplified graphical representation of the data model is used in {{ietf-l3-ospf-topology-tree}} of this document.
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

# YANG Data Model for OSPF Topology

The abstract (base) network data model is defined in the "ietf-network" module of {{!RFC8345}}. The OSPF-topology builds on the network data model defined in the "ietf-network" module {{!RFC8345}}, augmenting the nodes with OSPF information, which anchor the links and are contained in nodes.

There is a set of parameters and augmentations that are included at the node level. Each parameter and description are detailed following:

* Network-types: Its presence identifies the OSPF topology type. Thus, the network type MUST be ospf-topology.
+ OSPF timer attributes: Identifies the node timer attributes configured in the network element. They are wait timer, rapid delay, slow delay, and the timer type (linear or exponential back-off).
- OSPF status: contains the neighbours' information.

A second set of parameters, along with augmentations, is included at the link and termination point level. Each parameter is listed as follows:

* Interface-type
+ Area ID
+ Metric
- Passive mode

{: #ietf-l3-ospf-topology-tree}

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
<CODE BEGINS> file "ietf-l3-ospf-topology@2023-10-23.yang"

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
    "IETF OPSA (Operations and Management Area) Working Group";
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

     Copyright (c) 2022 IETF Trust and the persons identified as
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
