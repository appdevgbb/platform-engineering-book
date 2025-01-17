### Managing Risks and Trade-offs in Platform Engineering
As software development continues to evolve at a rapid pace, platform engineering has become a transformative force. By creating scalable, standardized, and developer-oriented environments, organizations can augment productivity and drive innovation. Yet, this isn't a straightforward path. Navigating platform engineering involves managing the complex balance of risks and trade-offs that can deeply influence its success and adoption in an organization. Common challenges can stem from over-customizating a solution to building vs buying a solution to choosing a specific vendor - and how to avoid vendor lock-in. In this section we will discuss each one of these topics and provide you with a mental model to guide better decision-making called Second Order Thinking.

#### What is Second-Order Thinking?

Second-order thinking is a mental model that enables you to look beyond the immediate outcomes of decisions, anticipating their longer-term and often less obvious consequences. While first-order thinking focuses on solving immediate problems, second-order thinking evaluates the ripple effects those solutions may create. This approach helps organizations anticipate risks, uncover hidden trade-offs, and make more informed decisions. It’s especially valuable in platform engineering, where choices like customization or tool selection can have far-reaching impacts on scalability, maintainability, and agility.

#### Over-Customization: A Double-Edged Sword
Customization is often seen as a way to cater to the unique needs of various teams. While this can enhance usability and team productivity in the short term, over-customization can create a maze of bespoke solutions that are difficult to maintain, debug, and scale. Each custom feature or tweak adds to the maintenance burden, creating a platform that may eventually become more of a liability than an asset. Organizations must strike a balance by implementing modular solutions and setting boundaries on customization to prevent technical debt from spiraling out of control.

Example of Second-Order Thinking: Customizing Open Source Projects

Consider an organization that decides to heavily customize an open-source platform to meet specific needs - e.g.: a team might want to fork the NGINX project and change it's source code so it fits their specific needs. This approach offers immediate benefits, such as highly customized workflows and improved developer experience. However, second-order thinking reveals deeper implications:

| **Decision** | **Immediate Effects (1st Order)** | **Second-Order Effects** | **Third-Order Effects** |
| - | - | - | - |
| Customize an open-source solution | Improved workflows and developer experience | (a) Platform diverges significantly from the original project, creating maintenance challenges. <br>(b) Limited ability to leverage community contributions such as security patches and innovations.<br>(c) Dependence on internal teams for innovation and delayed access to critical updates. | (a) Long-term isolation from community updates and tools, leading to increased operational complexity<br>(b) Added complexity on day 2 operations and need to fully support a forked solution.<br> (c) Potential issues with break changes from the original branch, creating a full one off solution that is now solely maintained by the organization.<br> (d) Developers might be spending more time maintaining the solution vs innovation new solutions and product.

#### Vendor Lock-In: The Cost of Convenience
The idea of best-in-class tools and services often leads to the adoption of specialized third-party solutions. However, as it was the case with over-customization, relying too heavily on specific vendors introduces the risk of vendor lock-in. As platforms grow and evolve, switching vendors or adapting to new technologies may become prohibitively expensive or complex. 

- Hypothesis: To mitigate this, platform engineers should prioritize tools that adhere to open standards and maintain interoperability with other systems. 

- Testing the hypothesis: A team of developers might decide that they want to have their database inside of a Kubernetes cluster with the mindset that this would allow them to:
    
    (a) be more cloud-agnostic, 
    
    (b) enhanced audit and logging capabilities, 
    
    (c) roll out their version of their database whenever they want to,

    (d) cheaper than the PaaS offer since they already pay for the cluster. 
    
These reasons sound very compeling. Let's use our Second Order Thinking model to expand these ideas:

| **Decision** | **Immediate Effects (1st Order)** | **Second-Order Effects** | **Third-Order Effects** |
| - | - | - | - |
| Use a database in-cluster instead of a PaaS offer | (a) cloud-agnostic solution,<br>(b) enhanced audit and logging capabilities,<br>(c) full control over the database, <br>(d) cheaper than the PaaS offer since they already pay for the cluster. | (a) Team is now responsible for backing up and maintaining the database performance (IOPS/Bandwidth), including applying security patches.<br>(b) As a stateful workload on Kubernetes, the team has to be maintain a backup strategy (RTO/RPO) for the persistent volumes that the database use.<br>(c) Team needs to take into account High Availability of the database, what type of underlying storage to use and wether that is zone redudant (e.g.: Zone Redundant Storage in Azure). <br> (d) Cluster upgrades might be restricted to an in-place upgrade vs blue-green and other approaches.| (a) Upgrading the cluster will need the sign-off between all of the various applications owned by product owners in the organization.<br>(b) BCDR is extremely challenging as the database is colocated with the cluster in a single region. Multi-region replication can take a toll on designing the solution and choosing the right approach: active/active, active/passive. Availability zones will help locally only.<br>(c) Complex Infrastructure as Code (IaC), smoke tests and thorough tests are required to safeguard the creation of clusters with stateful workloads that need to have their volumes rehydrated - e.g.: using in-cluster backup solutions such as Velero or cloud provider solutions (e.g.: Azure Backup).

Although this hypothesis makes sense and seem very reasonable, there is one aspect of it that can easily backfire if not well thought out. When organizations try to embrace more responsibilities than they can effectively take ownership.

#### Flexibility vs. Standardization: Walking the Tightrope
A core tenet of platform engineering is to provide developers with the flexibility they need to innovate while maintaining a standardized platform that ensures efficiency and reliability. However, achieving this balance can be challenging. Excessive standardization can stifle creativity and force developers into rigid workflows that may not suit their needs. On the other hand, too much flexibility can lead to inconsistencies, fragmented toolchains, and reduced efficiency. The key lies in designing a platform that offers standardized core services while allowing teams to extend or adapt these services within defined parameters.

#### Best Practices for Navigating Trade-offs
Successfully managing these risks and trade-offs requires a proactive and strategic approach. Here are some best practices:

1. **Adopt a Modular Architecture**: Build the platform in a way that allows for easy integration and replacement of components. This reduces the impact of vendor lock-in and enables incremental updates.
2. **Encourage Feedback and Collaboration**: Engage with development teams regularly to ensure the platform meets their needs without excessive customization.
3. **Define Clear Boundaries**: Establish guidelines on what can be customized and what should remain standardized, providing clarity to all stakeholders.
4. **Invest in Training and Documentation**: Equip teams with the knowledge to use the platform effectively, reducing reliance on bespoke solutions.
5. **Evaluate Tools with Long-Term Goals in Mind**: Consider not only the immediate benefits of a tool but also its adaptability, compatibility, and potential exit strategies.