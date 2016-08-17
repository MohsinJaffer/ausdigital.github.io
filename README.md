# Purpose of this Site

Exchange of buisness documents like invoices between business finance systems ought to be as simple as sending and email.  This site provides some open specifications and test services designed to make that easy.
* As a business, I want to send electronic invoices to my customer systems and know the payment status so I can manage my cashflow and get cheap debtor financing if I need it.
* As a business I want all invoices and tax receipts from my authorised suppliers to be automatically loaded to my financial system ready for my approval so I can reduce my administration burden.

To support that goal, this site provides
* A suite of technical specifications - including [identity-provider](#The Identity Provider Specification), [capability-lookup](#The Digital Capability Locator Specification), [metadata-publisher](#The Service Metadata Publisher Specification), [access-point](#The Access Point Specification), and [notary](#The Notary Specification)
* A suite of semantic specifications -  initially [billing-semantics](#Billing Semantics Specification) and, in future, [order-semantics](#Purchase Order Semantics Specification), [shipping-semantics](#Shipping Notice Semantics Specification)
* A set of special interest groups including [development](#Development and Testing Interest Group), [engagement](Engagement Interest Group), [infoSec](#Information Security and Privacy Interest Group), [ledger services](Ledger Services Interest Group), and [trade finance](Trade Finance Interest Group)
* An open and transparent [governance and participation](#Specification Governance model) model that performas all work in the public domain and is open to participation by intrested parties.

# Background

The [Australian Digital Business Council](http://digitalbusinesscouncil.com.au/) has promised to publish an interoperability framework that aims to increase national productivity through automation of common buisness processes such as invoicing.  We anticipate that the published framework will include;
* A discovery model based on OASIS BDX and SMP standards
* A messaging model based on OASIS ebMS3/AS4 standards
* A semantic model based on OASIS UBL 2.1 standards
We will link to them when they are published.  

The Digital Business Council has also created a RESTful working group that aims to provide a simpler and more secure implementation model based on ubiquitous internet standards such as REST.  This site is the repository for those specifications.  

# How it Works

Unlike single provider APIs (eg google or facebook), a B2B community needs many different systems to implement the same service interfaces (eg an e-invoicing end-point) so that any business can send any other business an invoice.  The many-to-many aspect of B2B transactions requires a few extra components in the framework as shown in the conceptual overview diagram.

![Framework Diagram](AusDigitalHomepage.png)

For any of the millions of buisnesses in an economy to be able to exchange electronic business documents with any other business:
* Buyers and sellers may have different ledger systems but they must expose access points that conforms to a common interface standard.
* Independent and trusted identity providers need to be able to confirm business identity (to avoid every business having to issue passwords to every other business)
* Electronic service end-point information needs to be discoverable via a registry because a sender system will generally know the identity of a receiver (eg ABN 1234) but not necessarily the technical data about how and where to send and electronic invoice.
* Security measures such as signatures, encryption, and notaries are essential to provide trust and reduce fraud opportunities.

The success of the interoperability framework depends on uptake by the ledger software providers. Those systems must implement a numebr of interfaces in a consistent way - which requires clear standards, good test services, and easy to use tooling.  That is the purpose of this site.

# The Technical Framework Specifications

There is one or more interface specifications for each of the interactions in the overview diagram.  

## The Identity Provider Specification

A federated set of one or more identity providers are the key source of trust for the network.  This specificaiton defines a standard taxonomy of OIDC claims and scopes, together with a set of identity assurance levels and a standard way to link identity claims.
* [IDP Githib Working Group](https://github.com/ausdigital/identity-provider)

| Specification URL | Version | Status | API Definition | Test Service | Comments |
| ----------------- | ------  | ------ | -------------- | ------------ | -------- |
| Readthedocs link | 1.0 | Draft  | Swaggerhub Link | testpoint.io link  |   |


## The Digital Capability Locator Specification

The framework assumes that there could be multiple SMP in the network and so the digital capability locator is essentially a DNS entry (NAPTR Record type) that is used to redirect a lookup query for a given business identifier to the correct SMP.

* [DCL Working Group](https://github.com/ausdigital/capability-locator)

| Specification URL | Version | Status | API Definition | Test Service | Comments |
| ----------------- | ------  | ------ | -------------- | ------------ | -------- |
| Readthedocs link | 1.0 | Draft  | Swaggerhub Link | testpoint.io link  |   |


## The Service Metadata Publisher Specification

The framework depends heavily on the ability to discover detailed service information for any given business identifier.  The SMP maintains a list of businesses, with a list of services for each business. Each service lists supported document formats and transport protocols and holds a digital certificate for message signing and encryption.

* [SMP Working Group](https://github.com/ausdigital/metadata-publisher)

| Specification URL | Version | Status | API Definition | Test Service | Comments |
| ----------------- | ------- | ------ | -------------- | ------------ | -------- |
| Readthedocs link | 1.0 | Draft | [SMP API v0.1](https://swaggerhub.com/api/ausdigital/smp/0.1)  | [smp.testpoint.io](https://smp.testpoint.io) |    |

## The Access Point Specification

The access point, usually provided by the ledger vendor, is the service gateway for a given business and so hosts the end-point to which business documents are sent.  The gateway is an avaialbility mediator (allowing specific businesses to have intermittent connectivity) and is responsible for signature validation and submitting transactions to the notary service.

* [AP Working Group](https://github.com/ausdigital/access-point])

| Specification URL | Version | Status | API Definition | Test Service | Comments |
| ----------------- | ------  | ------ | -------------- | ------------ | -------- |
| Readthedocs link | 1.0 | Draft  | Swaggerhub Link | testpoint.io link  |   |

## The Notary Specification

The notary serivce provides an irrefutable history of a specific "contract" (eg an invoice and it's status lifecycle), recorded in a blockchain distributed ledger.  This service supports dispute resolution and provides the foundation platform for financial services such as debtor financing.

* [NRY Working Group](https://github.com/ausdigital/notary)

| Specification URL | Version | Status | API Definition | Test Service | Comments |
| ----------------- | ------  | ------ | -------------- | ------------ | -------- |
| Readthedocs link | 1.0 | Draft  | Swaggerhub Link | testpoint.io link  |   |

# The Business Document Semantic Specifications

The technical framework exists only to provide the foundation for the secure and reliable interchange of business documents such as electronic invoices.  The specifications in this section describe the semantics of each business document.  Note that;
* the semantics can vary depending on the process context - for example "commercial invoice" and "reciptient created tax invoice" are two different processes that leverage the same document type.   
* each process has at least two roles (eg Buyer and Seller) with different requirements under the specification.  

Therefore the semantic specifictions group compliance requirements by process and role.

Like the technical specifications, the semantic specification are developed using the [COSS](http://rfc.unprotocols.org/spec:2/COSS/) (Consensus Oriented Specification System) governance model.

## Billing Semantics Specification

The Billing Specifications are based on [OASIS UBL2.1](http://docs.oasis-open.org/ubl/UBL-2.1.html) [billing semantics](http://docs.oasis-open.org/ubl/os-UBL-2.1/UBL-2.1.html#S-BILLING) and include the Invoice, Credit Note, Debit Note, and Application Response documents.  The process roles are buyer and seller.

* [Billing Working Group](https://github.com/ausdigital/billing-semantics)

| Specification URL | Version | Status | API Definition | Test Service | Comments |
| ----------------- | ------  | ------ | -------------- | ------------ | -------- |
| Readthedocs link | 1.0 | Draft  | Swaggerhub Link | testpoint.io link  |   |

## Purchase Order Semantics Specification

Coming soon - let us know if you'd like to lead this group.

## Shipping Notice Semantics Specification

Coming soon - let us know if you'd like to lead this group

# The Special Interest Groups

Some interest groups cross multiple specifications and may have an impact on many of them but are not, in themselves, the logical owners of a particular specification.  We value the support and opinions of such groups and so we provide some collaboration tools to facilitate consultation and collaboration with such groups.  If you'd like to participate (or just watch the conversation) of a particular group then please follow the links to subscribe to the chat channel or low volume mailing list.  If you'd like to create a new special interest group, please join the engagement group and raise your request there.

## Development and Testing Interest Group

A special interest group for implementers that would like to share their experiences and get help from a community of implementers. 
* Join the [development discussion group](http://talk.testpoint.io/outreach-sig/channels/sig-development)
* Subscribe to the [development mailing list]( )

## Engagement Interest Group

A special interest group that is focussed on stakeholder engagement and outreach in order to ensure that all interested parties are aware and have the opportunity to participate.
* Join the [engagement discussion group](http://talk.testpoint.io/outreach-sig/channels/sig-engagement)
* Subscribe to the [engagement mailing list]( )

## Information Security and Privacy Interest Group

A special interest group for those concerned about information security and privacy issues related to the specifciations.
* Join the [infosec discussion group](http://talk.testpoint.io/outreach-sig/channels/sig-infosec)
* Subscribe to the [infosec mailing list]( )

## Ledger Services Interest Group

A special interest group for financial software vendors to discuss an issues related to ledger systems and their support for the specification.
* Join the [ledger services discussion group](http://talk.testpoint.io/outreach-sig/channels/sig-ledger-services)
* Subscribe to the [ledger services mailing list]( )

## Trade Finance Interest Group

A special interest group for financial software vendors to discuss an issues related to ledger systems and their support for the specification.
* Join the [trade finance discussion group](http://talk.testpoint.io/outreach-sig/channels/sig-trade-finance)
* Subscribe to the [trade finance mailing list]( )

# Specification Governance model

All technical specifications are developed using the [COSS](http://rfc.unprotocols.org/spec:2/COSS/) (Consensus Oriented Specification System) governance model.  COSS ensures that any interested party can participate (because it imposes no restrictins on contributions). It also maximises consensus because, if the editor does not accept your contribution, you can always fork the specification and become the new editor (if others agree they'll follow, but if nobody else likes your fork then you'll be editor of a lonely specification that will wither on the vine).

## Participation

All specifications are developed in the public domain.  To make your voice heard, you can
* Raise a ticket on any of the relevant repositories
* Participate in a special interest group.

A new technical specification can be launched at any time via the following process:
1. [Raise a ticket](https://github.com/ausdigital/ausdigital.github.io/issues) in with title “Proposed SPEC”.  Document a high level scope and purpose of the proposed spec in the issue.
2. Prove evidence of participation interest by collecting "+1" in the form of comments on the issue from at least 6 other people from at least 3 different organisations. 
3. Nominate an Editor
4. Indicate acceptance of the [COSS](http://rfc.unprotocols.org/spec:2/COSS/) governance model and the [http://semver.org/](SemVer) versioning model for specification development.
5. Once steps 1 to 4 are complete, we will create the repository, give the editor write access, and add the default document template
6. The new editor should then notify relevant special interest groups of issues or questions regarding the new SPEC

## Implementation Compliance Statement for technical specifications

As a software application vendor seeking compliance with the framework you;
* MUST support the AP specification, and
* MUST support the client role in the DCL specification, and
* MUST support the client role in the SMP specification, and
* SHOULD support the RP (Relying Party) role in the IDP specification, and
* MAY support the client role in the NRY specification, and
* MAY choose to outsource these specification compliance requirements to a compliant third party
* 
## Compliance Statement for semantic specifications

As a software application vendor seeking compliance with the framework you;
* SHOULD support one or more of the business document semantic specifications, and
* MAY choose to support one or more processes for the given business document, and
* MAY choose to support one or more roles (eg buyer or seller) in the relevant process, and
* MUST publish all supported semantic documents, processes, and roles to an SMP


