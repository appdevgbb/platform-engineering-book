### Platform Engineering as a Stack

A key concept within platform engineering is viewing the platform as a stack—a structured, layered approach that aligns tools and technologies with the needs of both the organization and its developers. This section will take you through the top-down view of the platform stack, breaking down its components and illustrating how each layer contributes to a seamless and reliable engineering platform. These examples are based on what we see as part of our jobs as Application Innovation Global Black Belts and they could serve you as a guide.

#### **The Top-Down View of the Platform Stack**

A platform stack is best explained as a hierarchy of interdependent layers, each serving a distinct purpose in delivering a cohesive developer and operational experience. Starting with the `Developer Control Plane`, which interfaces directly with the developers, and ending with the` Resource Plane`, which provides the foundational infrastructure, this structure enables organizations to build platforms that are both robust and developer-friendly. Below is a summary of these layers, their purposes, and examples of technologies and tools typically used at each level.

| **Layer** | **Purpose** | **Examples**
|-|-|-
| **Developer Control Plane**| Provides tools and interfaces for developers to interact with the platform. | IDEs (VS Code, Codespaces), CLI Tools (Azure Developer CLI), Developer Portal (Backstage), GitHub |
| **Integration and Delivery Plane** | Enables seamless application integration and delivery. | CI Pipelines (GitHub Actions), CD Pipelines (Flux v2), Image Registry (Azure Container Registry) |
| **Monitoring and Logging Plane**  | Ensures system reliability through observability and issue resolution. | Prometheus, Azure Managed Grafana, Azure Monitor |
| **Security Plane** | Safeguards the platform with tools for secrets, policies, and network security. | Secrets Management (Azure Key Vault), Policy Management (Azure Policy), Network Security (Cilium) |
| **Resource Plane** | Provides the foundational compute, data, and networking resources. | Compute (Azure Kubernetes Service), Data (Azure SQL, Azure PostgreSQL), Networking (Azure DNS, Azure Traffic Manager), Integration Services (Azure Service Bus, Azure API Management)


##### **1. Developer Control Plane**
At the top of the platform stack, the developer control plane ensures that developers have intuitive tools and interfaces to interact with the platform. This layer includes:

- **IDE:** Integrated development environments like `VS Code` and `Codespaces`, providing developers with robust coding environments that developers are familiar with.

- **CLI Tools:** Command-line interfaces like Azure Developer CLI (`azd`) that streamline access to platform capabilities. Developers that are familiar with `terraform` could also leverage that.

- **Developer Portal:** Portals like `Backstage` that centralize access to documentation, services, and tools.

- **Version Control:** Tools like `GitHub` for managing source code and facilitating collaboration.Under `version control` we can find the `platform source code` and `application source code`. In here, there is a clear separation of duties between developers (`application`) and the platform engineering team (`platform`) and the tools by these teams.

    * Platform Source Code: Terraform and automations through GitHub Actions and Azure Automate.
    * Application Source Code: Typically divided into two categories: (1) application specs (typically defined in YAML, Helm Charts, Kubernetes manifests) and (2) actual source code (python, C#, Java, go, etc).

This layer focuses on simplifying the developer experience, increasing productivity, and reducing cognitive load.

##### **2. Integration and Delivery Plane**
This layer enables seamless integration and delivery of applications and services. Key components include:
- **CI Pipelines:** Tools like `GitHub Actions` for automating build and integration processes.
- **CD Pipelines:** Solutions like `Flux v2` for continuous deployment to `Kubernetes` environments.
- **Image Registry:** Registries such as `Azure Container Registry` for storing and managing container images.

The integration and delivery plane ensures smooth workflows, reducing friction in the software delivery lifecycle.

##### **3. Monitoring and Logging Plane**
Observability tools are vital for maintaining system reliability and identifying performance issues. This layer includes:

- **Observability Tools:** Managed solutions like `Prometheus`, `Azure Managed Grafana`, and `Azure Monitor` to provide comprehensive monitoring and logging capabilities. It is important to note that we are advocating for the use of PaaS services whenever possible, and that are two main reasons for that: (1) a fully managed service such as Azure Managed Grafana, will free up resources from the setup and configuration that is needed to run it in production and (2) managed services will often provide a high availability SLA (typically at least three 9's). Azure published that information [here](https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services?lang=1).

By enabling real-time insights into system health, this layer supports proactive issue resolution and continuous optimization.

##### **4. Security Plane**
Security is a critical aspect of any platform stack, and this layer ensures that systems remain secure and compliant. Key elements include:

- **Secrets Management:** `Azure Key Vault` for securely managing sensitive information such as API keys and credentials. Secrets, certificates and sensitive configuration can be stored in Key Vault and then projected into an AKS cluster by using the Azure Key Vault provider for Secrets Store CSI Driver.

- **Policy Management:** Enforcing compliance and governance policies using tools like `Azure Policy` and cloud-native specific tools like `Open Policy Agent`. Policies can be defined at a higher level in Azure Policy, using one of the various pre-canned policies, or they can be fully customized, using Rego, the policy language used by the `Open Policy Agent`.

- **Network Security:** Solutions like `Cilium` for securing network communication between services in an `Kubernetes` cluster and a network level firewall such as `Azure Firewall` for external facing connections.

The security plane safeguards the platform, ensuring data integrity and regulatory compliance.

##### **5. Resource Plane**
At the foundation of the stack, the resource plane provides the underlying compute, data, and networking resources. Components include:

- **Compute:**  `Azure Kubernetes Service (AKS)` for containerized workloads.

- **Data:**  `Azure SQL` and `Azure PostgreSQL` for structured data storage.

- **Networking:** Tools like `Azure DNS `and `Azure Traffic Manager` for managing connectivity. Developers might need a new A Record or CNAME for an application or new API endpoint and that can be provided by `Azure DNS`. This can further be further automated by deploying `ExternalDNS` to `Azure Kubernetes Services` and dynamically update `Azure DNS` with new DNS records as new services are created in the Kubernetes cluster. Traffic Manager on the other hand can be used to test geo-location based traffic patterns and high availability/failover.

- **Integration Services:** Services like `Azure Service Bus` and `Azure API Management` for connecting and managing application components.

The resource plane provides the raw capabilities upon which the entire stack is built, ensuring scalability and reliability.

#### Best Practices for Platform Engineering as a Stack
Platform engineering as a stack provides a structured approach to building and managing engineering platforms. Each layer—from the developer control plane to the resource plane—plays a crucial role in delivering a reliable, scalable, and efficient platform. 

By understanding and optimizing each layer, organizations can create a solution that:

* enables developer productivity by allowing them to leverage the tools they already know such as `VS Code`,

* accelerates innovation by spending less time in setting up environments and by leveraging environments that have been fully vettoed by a team of subject matter experts under the Platform Engineering group,

* and drives business success by allocating more resources into actual product development vs dealing with engineering a development platform.