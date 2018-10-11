# How to Architect a Multicloud-Capable Hybrid Integration Platform

------



As more applications and data move to the cloud, application leaders face increasingly complex integration requirements: within the same cloud, across different clouds and with on-premises endpoints. Supporting these diverse topologies in your hybrid integration platform strategy has become crucial.



## Overview



### Key Challenges

- Application leaders are increasingly dealing with applications and data sources scattered across multiple providers' cloud environments and structures (IaaS, PaaS and SaaS), which pose complex intracloud, intercloud and on-premises integration issues. Lacking familiarity with these issues, many application leaders are uncertain about how to address multicloud service integration (multi-CSI) needs.
- Different integration platform providers support diverse ways to tackle multi-CSI, which makes it difficult for application leaders to match these offerings with their requirements.
- Although integration platforms are becoming more versatile, few can address all of the requirements in organizations with a wide range of highly differentiated multi-CSI scenarios.



### Recommendations

Application leaders responsible for integration strategy and infrastructure should:

- Address multi-CSI strategically by explicitly including support for the four topologies — ground-to-ground, intracloud, cloud-to-ground and intercloud — in your integration platform plans.
- Implement a multicloud-capable hybrid integration platform (HIP) that enables the four topologies by selecting and assembling building blocks that support at least the remote access and distributed deployment models for integration platforms.
- Empower maximum multi-CSI flexibility in your HIP implementation using a federated combination of iPaaS and classic integration software, if you have demanding ground-to-ground topology requirements.



## Strategic Planning Assumption

By 2021, at least 75% of large and global organizations will implement a multicloud-capable hybrid integration platform, up from less than 25% in 2018.



## Introduction

As cloud adoption proliferates, some organizations are proactively pursuing a multicloud strategy. Many others find themselves "accidentally" with applications and data sources in multiple clouds, while maintaining a substantial portfolio of on-premises systems. No matter their approach to multicloud, for a growing number of organizations the issue of cloud service integration (CSI) is becoming more and more complex.

> Application leaders increasingly face the challenge of making applications and data sources, deployed in multiple cloud environments and in their own on-premises data centers, work together.

Let's consider the following scenario. A company has its CRM and marketing SaaS applications delivered by one cloud service provider, its ERP and HCM applications delivered by another cloud provider, and its data warehouse (DW) and warehouse management system (WMS) deployed in its on-premises data center (see Figure 1). These applications must be integrated in various ways to support different business needs — for example:

- The marketing application must share sales leads with the CRM application (see "a" in Figure 1)
- The CRM system must be able to exchange purchase orders with the ERP system ("b")
- The ERP application must receive employee data for accounting from the HCM system ("c") and integrate with the WMS for logistical purposes ("d").
- The CRM, ERP and WMS applications must feed the DW for reporting and analytics ("e," "f" and "g").





Figure 1. The Multi-CSI Challenge

Note: The red, gray and yellow integrations denote provision by three separate integration platform providers.

Source: Gartner (May 2018)

![The Multi-CSI Challenge](https://www.gartner.com/resources/344800/344836/344836_0001.png?reprintKey=1-532P6RL)

Supporting a scenario like this might lead one toward the use of multiple integration platforms:

- **Red integrations —** The company's marketing and CRM SaaS both come from Oracle (Oracle Marketing Cloud and Oracle Sales Cloud, respectively). It may be logical to use the vendor's iPaaS — Oracle Integration Cloud Service — to integrate the two applications with each other (see "a" in Figure 1). This iPaaS could also feed CRM data into the on-premises DW ("e") and possibly into your ERP environment, provided by a different vendor (SAP, "b").
- **Gray integrations —** The company's ERP and HCM SaaS both come from SAP (S/4HANA Cloud and SAP SuccessFactors, respectively). It could use SAP's iPaaS — SAP Cloud Platform Integration — to integrate the two products with each other ("c"). This iPaaS could also integrate the ERP application with the WMS and on-premises DW ("d" and "f"), and with Oracle Sales Cloud, as an alternative to the Oracle Integration Cloud ("b").
- **Yellow integration —** Finally, the company has a third platform — Informatica PowerCenter — to feed WMS data into the DW ("g").

Even in this simplified scenario, by tactically addressing multi-CSI in a locally optimized way, the company has introduced three integration platforms and a lot of complexity (for example, it might use three different integration mechanisms to push data into its DW).

By following this approach in a more complex real-life scenario, we would rapidly create a chaotic environment made of multiple, redundant integration platforms. There would be significant overlap in functionality, and obvious cost, manageability and skills implications. With each additional platform added to the mix, the challenges become increasingly difficult to address.

> Application leaders should architect their multi-CSI strategy in a way that minimizes the number of integration platforms. This approach will help keep skills requirements and costs at bay while enabling holistic administration, monitoring and management.

Ideally, a single platform that can run integration flows across on-premises and diverse clouds, thus providing a single integration development toolset and a single control pane, could achieve these goals. Although technically possible to achieve, however, this may not be a realistic proposition for organizations with complex integration challenges.

Many integration platform providers are trying to come up with a coherent integration platform capable of fully supporting multi-CSI requirements. Several are still working hard to consolidate multiple generations of technologies, such as ESBs, data integration tools, B2B gateway software, MFT platforms, iPaaS offerings and API management platforms. They may not yet have reached the optimal combination of functionality and even maturity and production readiness for all the component parts, meaning that the one-vendor approach remains suboptimal.

Application leaders, therefore, will have to face multiple trade-offs to be able to address the multi-CSI issue, in particular if they have sizable investments in classic integration software in place.

This research helps application leaders responsible for integration strategy and infrastructure to sort out these trade-offs. It dissects the multiple facets of the issue and discusses the characteristics an integration platform must have to support multi-CSI requirements.



## Analysis



### Include Support for Multicloud Topologies in Your Strategic Integration Platform Plans

Multi-CSI can look very intricate and desperately hard to grasp, but it boils down to a relatively simple set of core requirements.

> No matter how complex your multi-CSI challenge is, you can describe it as a combination of four basic topologies: ground-to-ground, intracloud, cloud-to-ground and intercloud.

Based on a hypothetical scenario with three endpoints — a CRM application, an ERP application and a DW — the four topologies are exemplified in Figure 2.





Figure 2. The Four Basic CSI Topologies

Source: Gartner (May 2018)

![The Four Basic CSI Topologies](https://www.gartner.com/resources/344800/344836/344836_0002.png?reprintKey=1-532P6RL)

The four topologies explained:

- **Ground-to-ground.** This topology represents the classic scenario where you have multiple on-premises applications and data sources that you must integrate with each other (see "a" in Figure 2).

  Many organizations have already addressed this issue via a combination of on-premises application integration platforms, data integration tools, message-oriented middleware, managed file transfer or other tools.

- **Intracloud.** This topology refers to a situation where you have several applications and data sources *all in the**same cloud environment* — for example, Alibaba Cloud, Amazon Web Services (AWS), Microsoft Azure, Google Cloud Platform, Oracle Cloud, Salesforce or SAP ("b").

  Many organizations have solved this problem by adopting an iPaaS, typically hosted in the same cloud environment as the applications to minimize data exchange latency and security issues.

- **Cloud-to-ground.** In this topology, some of your applications are running in one cloud environment and some are running in your on-premises data center ("c").

  Initially, organizations addressed this problem by leveraging their incumbent on-premises integration platform. Now a growing number of application leaders are adopting iPaaS for cost, convenience and fast time to integration reasons (see "Technology Insight: Enterprise Integration PaaS").

- **Intercloud.** In this topology, your cloud applications are deployed in *multiple, distinct cloud environments* ("d").

  This is still the most unexplored topology, but it can usually be addressed by leveraging iPaaS offerings.

> Your integration platform strategy must be architected to support all the topologies that are potentially relevant for you according to priorities dictated by their relative importance, in terms of amount of integration projects.

You may not necessarily need to enable all the topologies. For example, if your organization has executed a cloud-only application strategy, you don't need to think about supporting the ground-to-ground topology.

Referring back to our scenario, we can see that it combines all the four topologies:

- Ground-to-ground — WMS to DW (see "g" in Figure 1)
- Intracloud — CRM to ERP ("b")
- Cloud-to-ground — CRM to DW ("e"), ERP to WMS ("d") and ERP to DW ("f")
- Intercloud — marketing to CRM ("a") and ERP to HCM ("c")

> We call "full multi-CSI" (aka "full multicloud") a scenario that combines all these different topologies.

*Action Items:*

- Analyze your short-, medium- and, if possible, long-term multi-CSI requirements, and prioritize their relevance.
- Assess whether your current integration platform strategy supports these requirements, and recalibrate your plans accordingly.



### Select HIP Building Blocks That Support a Range of Hybrid Deployment Options

A growing number of organizations are facing an ever-expanding set of integration needs and complexities. In most cases, however, they are finding that an integration delivery model carried out by a centralized competency center cannot address integration challenges with the agility and short time-to-value mandated by the digital era.

To tackle this problem strategically, many application leaders responsible for integration are evolving their strategy to a model where "ad hoc" and "citizen" integrators in LOBs, subsidiaries and departments are empowered to perform certain integration task by themselves (see "Integration Personas and Their Impact on Integration Platform Strategy").

A key enabler of this strategy is a shared platform that can support a variety of integration domains and can be used by multiple constituencies. Gartner has formalized the capabilities of such an infrastructure into a framework called the hybrid integration platform (HIP — see "Innovation Insight for Hybrid Integration Platforms" and "How to Implement a Truly Hybrid Integration Platform").

Application leaders correctly focus the technology selection process for their HIP implementation on functionality, but often forget to consider multi-CSI support requirements. This is usually because they may not have a full understanding of what these requirements are and how they will evolve.

> It is key that you select HIP building blocks that support hybrid deployment options, to be able to address the four basic CSI topologies.

"Where" the integration platform runs may be, in fact, quite important for the success of your integration strategy. For example:

- If you have to integrate multiple applications all hosted on AWS, it would make a lot of sense to consider an integration platform running in the same cloud platform — not in your data center.
- Vice versa, if your applications are still primarily on-premises, it would make sense to integrate them through a platform deployed in your data center — not in a cloud environment.

But, what does "hybrid deployment options" really mean? What actually are these options?

If we consider Gartner's HIP capability framework, we can think of any integration platform functionality as grouped into three macrocomponents: the control pane, integration runtime and agents (see Figure 3).





Figure 3. Any Integration Platform Can Be Divided Into Three Macrocomponents

Source: Gartner (May 2018)

![Any Integration Platform Can Be Divided Into Three Macrocomponents](https://www.gartner.com/resources/344800/344836/344836_0003.png?reprintKey=1-532P6RL)

The three macrocomponents explained:

- **Control pane.** This macrocomponent provides development and management tools that support the role-based user experience, governance and operations capabilities of the platform, including security, management, monitoring and administration of the other components. Colloquially at times we refer to the control pane as the "mothership."

- **Integration runtime.** This executes the actual orchestration, transformation, mapping, data delivery and data quality aspects of the integration flows, typically by interpreting metadata or executing code downloaded from the control pane. Several platforms — for example, many iPaaS offerings — enable multiple distributed integration runtimes to be connected to the same mothership.

- **Agents.** These take care of connectivity and security issues. The agents are the runtime components where protocols and adapters are executed, and where certain security issues are taken care of (for example, in API management platforms the agent corresponds to the functions performed by microgateways). Some integration platforms, such as classic ESBs, incorporate the agents in the integration runtime, but in several iPaaS offerings, agents can be deployed remotely from the runtime itself.

  Together, the runtime executive and the agents execute the integration flows by performing orchestration, data transformation and mapping, and by connecting to endpoints via a variety of communication styles and protocols.

This "anatomy" is a reasonably accurate representation of the way in which many providers have implemented their offerings to support a plurality of CSI requirements. However, not all integration platforms on the market exactly map to this. You need to pay attention to the terminology used by providers: when referring to an "integration runtime" deployed in a distributed fashion, they may call it "agent," "local agent" or "remote agent."

It is important for you, therefore, to ascertain how the providers you are considering allow you to deploy the functionality described above, rather than focusing on the terminology they use. That is, ask not whether the platform supports "agents," but if it allows you to deploy the adapter runtime environment in a distributed, multicloud fashion.

An integration platform architected as described in Figure 3 may give application leaders the opportunity to support multiple deployment options: centralized, remote access and distributed (see Figure 4).





Figure 4. Integration Platform Deployment Models

Source: Gartner (May 2018)

![Integration Platform Deployment Models](https://www.gartner.com/resources/344800/344836/344836_0004.png?reprintKey=1-532P6RL)

The deployment options explained:

- **Centralized.** All the components of the platform are statically hosted in "one place," whether one particular cloud or your on-premises data center (see "a" in Figure 4).

  All iPaaS and most API management offerings support the centralized cloud-based model. Classic integration platform software usually supports the centralized on-premises model, but a growing number also enable deployment on IaaS environments. Vendors often do so by means of containerized — typically Docker-enabled — versions of their products, therefore can also support the centralized cloud-based model.

  *See Note 1 for examples of integration platform software supporting Docker deployments.*

- **Remote access.** The control pane and the integration runtime are statically hosted in "one place" — typically a cloud environment. The agents can be deployed in different, heterogeneous locations — on-premises or in different cloud environments ("b").

  This is a topology supported by several iPaaS offerings.

  *See Note 2 for examples of iPaaS supporting the remote access model.*

- **Distributed.** The control pane is statically hosted in "one place," but the integration runtime and the agents can be deployed in multiple, heterogeneous environments ("c").

  "Dockerized" on-premises integration platforms support this deployment model, as do several iPaaS offerings (although not all). In practice, such an iPaaS offering enables you to deploy an instance of the integration runtime in your data center, so that integration flows connecting multiple endpoints in the same data center are executed locally. Metadata and operation/monitoring data, but no application data, move from/to the mothership in the iPaaS provider's cloud to/from the distributed integration runtimes. Similarly, you can deploy other instances of the runtime executive in different cloud environments (for example, AWS or Microsoft Azure) to support "intracloud service integration" within endpoints in that environment.

  *See Note 3 for examples of iPaaS supporting the distributed model.*

> The choice of which deployment models your integration platform must support depends on the multi-CSI topologies you must enable.

By their very characteristics, in fact, each deployment model better supports certain topologies than others — summarized in Figure 5 and explained in detail below.





Figure 5. How Integration Platform Deployment Models Support CSI Topologies

Source: Gartner (May 2018)

![How Integration Platform Deployment Models Support CSI Topologies](https://www.gartner.com/resources/344800/344836/344836_0005.png?reprintKey=1-532P6RL)

**Centralized On-Premises Deployment Model**

*The entire integration platform instance is deployed on-premises.*

*Benefits:* This naturally supports well the ground-to-ground topology (all the involved endpoints are on-premises) and may support cloud-to-ground requirements. However, usually on-premises integration platforms provide adapters only for the most popular SaaS applications.

*Drawbacks:* In general it is not a good fit for the intercloud topology, where you must move data from one cloud endpoint to the other. This process, in fact, implies moving data from a source endpoint in one cloud environment to the on-premises agents and runtime environment for mapping, transformation and orchestration, then back to a destination endpoint in the same cloud environment as the source (see "a" in Figure 6).

Although it can technically work, this scenario may have negative latency and throughput implications if the number of endpoints connected and/or the amount of data exchanged is significant. Moreover, it has security implications that must be addressed — for example, data in transit from the on-premises and the cloud environments must be encrypted and must traverse the company firewall.

For the same reasons, this deployment model it is not recommended to support a full multicloud topology, unless you have to integrate only a few endpoints.





Figure 6. How Centralized and Remote Access Deployment Models Support the Ground-to-Ground Topology

Source: Gartner (May 2018)

![How Centralized and Remote Access Deployment Models Support the Ground-to-Ground Topology](https://www.gartner.com/resources/344800/344836/344836_0006.png?reprintKey=1-532P6RL)

**Centralized Cloud Deployment Model**

*The entire integration platform instance is deployed in a cloud environment.*

*Benefits:* This model works well with:

- Intracloud requirements involving endpoints in the same cloud environment as the integration platform itself
- Intercloud integration and cloud-to-ground scenarios

*Drawbacks:* It is not well-suited to supporting demanding ground-to-ground requirements, for latency and throughput reasons symmetrical to those discussed above (see "b" in Figure 6).

Moreover, it may be technically complex to implement when the only way you can connect to some endpoints is via non-cloud-friendly protocols. For basically the same reasons, it is not very suitable for intracloud scenarios involving endpoints located on a different cloud environment. Ultimately, therefore, it is not recommended for complex full multicloud scenarios.

**Remote Access Deployment Model**

*Benefits:* Has similar applicability to the centralized cloud deployment model. However, by enabling the agents to be deployed on-premises or in different cloud environments, it makes it easier to connect endpoints that do not support cloud-friendly protocols. This is because the agent can run an adapter that implements these protocols. The agent then sends the data to the integration runtime (in the cloud) via a cloud-friendly protocol such as HTTP-S (see "c" in Figure 6).

Agents typically implement secure communication between the on-premises and cloud environments without needing to open ports in your firewall. This is because they usually encrypt data in transit and can be configured to communicate with your proxy server or support TLS socket tunnels.

*Drawbacks:* Like the centralized cloud model, remote access is not well-suited to supporting demanding ground-to-ground requirements, nor is it very suitable for intracloud scenarios involving endpoints located in a different cloud environment.

**Distributed Deployment Model**

*Benefits:* This is much more flexible than the centralized and remote access models, and can support basically all the topologies, although some better than others. For example, a distributed-model iPaaS supports intracloud, intercloud, ground-to-ground and cloud-to-ground requirements well (see Figure 7).

On-premises (or local) integration runtimes communicate with the cloud-based mothership via cloud-friendly protocols. They implement cloud-to-ground secure communication by using the same techniques used by local agents.

*Drawbacks:* A distributed model iPaaS, however, is not always the optimal choice when it comes to supporting highly demanding ground-to-ground needs. For example, many iPaaS offerings are not yet well proven to support the processing of millions of messages per day in a ground-to-ground scenario. Similarly, dockerized integration platform software may not be the best choice to support intracloud or intercloud scenarios due to the limited number of SaaS and cloud service adapters they provide.





Figure 7. Distributed iPaaS Deployment Supports Full Multi-CSI

Source: Gartner (May 2018)

![Distributed iPaaS Deployment Supports Full Multi-CSI](https://www.gartner.com/resources/344800/344836/344836_0007.png?reprintKey=1-532P6RL)

*Action Items:*

- If your application and data sources portfolio is centered on one particular cloud environment, select HIP building blocks that can be hosted in that particular cloud environment.
- If your application and data sources portfolio are hosted in multiple cloud environments, possibly also including on-premises scenarios, select HIP building blocks that enable distributed — or at least remote access — deployment models.



### Use in Your HIP a Federated Combination of iPaaS and Classic Integration Software for Maximum Flexibility

Over the past 10 to 15 years, many organizations have already notably invested in classic on-premises integration platform software, typically to support ground-to-ground integration requirements.Often these platforms support hundreds or thousands of integration flows across dozens — if not hundreds — of applications and data sources.

As already noted, these platforms can also support cloud-to-ground requirements, but are not well-suited for more cloud-centric scenarios. This is because they don't typically support the remote access or distributed deployment models (albeit with exceptions, such as SAP Process Orchestration). However, for application leaders in these organizations, the notion of replacing their established integration platform software with iPaaS offerings to better support multi-CSI is simply unthinkable. It would be expensive, risky to implement and very difficult to justify as the perceived business benefit would be minimal.

Moreover, as already noted, iPaaS offerings — including those supporting a distributed deployment model — are often not yet well-established solutions for demanding ground-to-ground requirements (in terms of message latency, throughput and scalability).

Finally, in complex and diversified organizations, a single iPaaS may not be sufficient to support the variety of integration use cases that may manifest in different departments or business units. For example, certain use cases may be focused on bulk data transfer, whereas others may be more targeted at real-time, API-style interactions.

The most practical approach to implementing your HIP infrastructure in these situations is likely to be a federation of multiple building blocks. These may include integration platform software (ESB, ETL tools), iPaaS, API management, B2B gateway software and iSaaS (see "Market Guide for HIP-Enabling Technologies").

In a federated HIP implementation model, you may have a multivendor scenario. For example, it may include your classic ESB deployed on-premises and one or more iPaaS offerings from different providers. Some of these iPaaS offerings, in turn, may be hosted in a distributed, multicloud environment (see Figure 8).





Figure 8. Federated HIP Deployment Supports Demanding Full Multi-CSI

Source: Gartner (May 2018)

![Federated HIP Deployment Supports Demanding Full Multi-CSI](https://www.gartner.com/resources/344800/344836/344836_0008.png?reprintKey=1-532P6RL)

The essential prerequisite is that these different platforms and instances can "work together" to enable multi-CSI. For example, ideally:

- The different iPaaS can consume APIs published by the ESB
- An integration flow running on the ESB can trigger another flow in one of the iPaaS environments
- The ESB can publish as an API an integration flow executed in one of the iPaaS environments

Implementing a heterogeneous federated model implies that you have to perform some integration work to:

- Certify the levels of interoperability that you need
- Harmonize monitoring, management and administration processes

However, several providers support certified out-of-the-box interoperability between their classic integration software and their iPaaS. (See Note 4 for examples of providers supporting out-of-the box interoperability between their integration software and iPaaS.)

Despite its complexity in terms of operations and its demanding skills requirements, the federated model is extremely powerful because it can effectively support even the most complex scenarios.

For example, a federated model combining an on-premises ESB and an iPaaS that supports the distributed deployment model would enable even the most intricate and diversified cloud-to-ground, intracloud and intercloud integration scenarios, as well as the most demanding ground-to-ground use cases.

*Action Item:*

- Adopt a federated model for your HIP implementation to support multi-CSI if you have either:
  - An established integration platform software that would be impractical to replace with modern iPaaS technologies.
  - A significant portfolio of on-premises applications and data sources, characterized by demanding ground-to-ground integration requirements in terms of latency, throughput or scalability, that must also integrate with a variety of cloud environments and endpoints.
  - A large number of highly differentiated integration scenarios, which would be challenging to support with a single integration platform.



## Acronym Key and Glossary Terms

| ESB   | enterprise service bus                 |
| ----- | -------------------------------------- |
| ETL   | extraction, transformation and loading |
| iPaaS | integration platform as a service      |
| iSaaS | integration software as a service      |
| LOB   | line of business                       |
| MFT   | managed file transfer                  |
| TLS   | Transport Layer Security               |



## Note 1Examples of Classic Integration Platform Software That Support Container-Based Deployment

- MuleSoft Anypoint Platform
- Red Hat JBoss Fuse
- SAP Process Orchestration
- Software AG webMethods Integration Cloud: Container Edition
- TIBCO BusinessWorks Container Edition



## Note 2Examples of iPaaS Offerings That Support Remote Access Deployment

- Celigo
- Cloud Elements
- Microsoft Azure Logic Apps
- SAP Cloud Platform Integration
- Workato



## Note 3Examples of iPaaS Offerings That Support Distributed Deployment

- DBSync Cloud Workflow
- Dell Boomi AtomSphere
- elastic.io
- HiQ Frends
- IBM App Connect Enterprise
- IConduct
- Informatica Intelligent Cloud Services
- Jitterbit API Integration Platform
- Magic Software Enterprises Magic xpc
- Moskitos Crosscut
- MuleSoft Anypoint Platform
- RoboMQ Hybrid Integration Platform
- SAP Cloud Platform Integration (via SAP Process Orchestration for Cloud Platform Integration runtime)
- SnapLogic Enterprise Integration Cloud
- Scribe Online



## Note 4Examples of Providers Supporting Out-of-the-Box Interoperability Between Their Integration Platform Software and iPaaS

- Adeptia (Connect and Adeptia Integration Suite)
- IBM (IBM Integration Bus and IBM App Connect Professional)
- Informatica (Informatica PowerCenter and Informatica Intelligent Cloud Services)
- Microsoft (Microsoft BizTalk Server and Azure Logic Apps)
- Oracle (Oracle SOA Suite and Oracle Integration Cloud)
- SAP (SAP Process Orchestration and SAP Cloud Platform Integration)
- Software AG (webMethods Integration Server and webMethods Integration Cloud)
- TIBCO (TIBCO BusinessWorks and TIBCO Cloud Integration)

![View Analyst Massimo Pezzini](https://www.gartner.com/images/headshots/color_285x285/9152.png)Massimo PezziniVP & Gartner Fellow



© 2018 Gartner, Inc. and/or its affiliates. All rights reserved. Gartner is a registered trademark of Gartner, Inc. and its affiliates. This publication may not be reproduced or distributed in any form without Gartner's prior written permission. It consists of the opinions of Gartner's research organization, which should not be construed as statements of fact. While the information contained in this publication has been obtained from sources believed to be reliable, Gartner disclaims all warranties as to the accuracy, completeness or adequacy of such information. Although Gartner research may address legal and financial issues, Gartner does not provide legal or investment advice and its research should not be construed or used as such. Your access and use of this publication are governed by [Gartner’s Usage Policy](https://www.gartner.com/technology/about/policies/usage_policy.jsp). Gartner prides itself on its reputation for independence and objectivity. Its research is produced independently by its research organization without input or influence from any third party. For further information, see "[Guiding Principles on Independence and Objectivity](https://www.gartner.com/technology/about/ombudsman/omb_guide2.jsp)."







![Gartner, Inc.](https://www.gartner.com/imagesrv/apps/common/images/logo-white.png)

© 2018 Gartner, Inc. and/or its Affiliates. All Rights Reserved.