# Day 39 — Virtualization, Containers and Serverless

> **CompTIA Security+ SY0-701** · **Week 08, Lesson 4 of 5** · Domain 3, Objective **3.1: Security implications of different architecture models** (related: 3.2 and 4.1)  
> **Level:** Beginner to intermediate · **Study time:** 60–80 minutes  
> **Previous:** [Day 38 — Cloud Models and Shared Responsibility](day-38-cloud-models-and-shared-responsibility.md)  
> **Next:** Day 40 — Title pending curriculum confirmation

## 1. Learning objectives

By the end of this lesson, you should be able to:

- Distinguish virtual machines, containers and serverless functions by their execution and isolation mechanisms.
- Compare Type 1 and Type 2 hypervisors and identify virtualization-specific risks.
- Explain container images, registries, runtimes, namespaces and resource limits.
- Interpret trust boundaries and identify shared components in a containerized application.
- Select controls for image integrity, workload isolation, secrets and runtime permissions.
- Explain how serverless execution changes operational responsibilities without eliminating application security obligations.
- Evaluate architectural trade-offs and diagram a layered containerized service.

## 2. The big idea: Sharing hardware without sharing everything

Imagine a company that needs to run a website, a database and an internal reporting application. Buying a physical computer for each service provides separation but consumes space, electricity and administrative time. Running everything directly on one operating system saves resources, but a vulnerable application might affect its neighbors.

Virtualization and containerization provide ways to share computing resources while establishing boundaries between workloads. Serverless computing goes further by transferring much of the execution infrastructure management to a service provider. None of these approaches removes the need to protect applications, identities and data.

| Architecture | What is packaged or provisioned? | Typical isolation boundary |
| --- | --- | --- |
| Virtual machine (VM) | Virtual hardware and guest operating system | Hypervisor and virtual hardware |
| Container | Application and user-space dependencies | Operating-system kernel mechanisms |
| Serverless function | Code, dependencies and execution configuration | Provider-managed execution environment |

These are not mutually exclusive categories. A cloud provider can run containers inside VMs and use containers or lightweight VMs to implement serverless services. The relevant question is which boundary protects a particular workload and who manages it.

🧠 **Memory hook:** A VM virtualizes a computer; a container isolates processes; serverless abstracts server management.

**CIA triad connection:** Isolation supports confidentiality and integrity by restricting cross-workload access. Resource limits, redundancy and recovery mechanisms support availability. A shared component can undermine all three if it fails or is compromised.

## 3. Virtualization and hypervisors

Virtualization allows multiple operating systems to execute on one physical host. A hypervisor mediates access to processor time, memory, storage and network interfaces. Each guest operating system sees virtual hardware and can operate independently of the other guests.

```text
VM A: application + guest OS     VM B: application + guest OS
                 |                              |
                 +---------- Hypervisor --------+
                               |
                        Physical hardware
```

The hypervisor maintains mappings between guest resources and physical resources. Hardware-assisted virtualization helps enforce memory and execution boundaries. Virtual network interfaces connect guests to virtual switches, which may connect to physical networks or remain isolated.

### Type 1 and Type 2 hypervisors

| Characteristic | Type 1 (bare metal) | Type 2 (hosted) |
| --- | --- | --- |
| Placement | Runs directly on hardware in the conventional model | Runs through a host operating system |
| Typical use | Data centers and production infrastructure | Desktop development and training labs |
| Examples | VMware ESXi, Microsoft Hyper-V | VirtualBox, VMware Workstation |
| Security dependency | Hypervisor and management components | Hypervisor plus host OS and its services |

Some modern products blur this classification through kernel integration or privileged management partitions. For exam purposes, distinguish bare-metal from hosted architecture rather than assuming every implementation has an identical stack.

### Virtual machine security risks

A **VM escape** occurs when code breaks out of a guest's intended isolation boundary and gains access to the hypervisor or host environment. It is not the same as gaining administrator privileges inside a guest. Hypervisor compromise can expose multiple workloads because the hypervisor mediates their resources.

**VM sprawl** occurs when organizations create VMs faster than they can inventory, patch and retire them. An abandoned VM might retain an exposed management service or outdated credentials. Automation makes deployment easy, but without lifecycle governance it also makes insecure deployment easy.

Snapshots preserve a point-in-time state of a VM. Depending on configuration, they may contain memory, disk contents and sensitive information. Reverting a snapshot may also restore vulnerable software or outdated configuration. Snapshots on the same storage system do not constitute an independent disaster recovery strategy.

| Risk | Appropriate controls | Limitation |
| --- | --- | --- |
| Hypervisor compromise | Patch hypervisor, restrict management access, monitor administrative actions | Does not patch guest applications |
| Guest compromise | Guest hardening, endpoint protection, patching | Does not guarantee hypervisor integrity |
| VM sprawl | Inventory, ownership tags, expiry and retirement policies | Inventory must remain current |
| Resource exhaustion | Reservations, quotas, monitoring and capacity planning | Cannot eliminate physical host failure |
| Snapshot exposure | Encryption, restricted access, retention policy | Encryption does not prevent authorized misuse |

**Exam trap:** A VM is not automatically an air-gapped system. Shared folders, virtual networking, clipboard integration and management interfaces may create paths across boundaries.

## 4. Containers: Images, runtimes and shared kernels

A container packages an application with the user-space libraries and files it needs. Unlike a conventional VM, a Linux container does not normally boot a separate guest kernel. Its processes use the kernel of the Linux environment in which the container runtime operates. On macOS and Windows, Linux containers commonly run inside a Linux VM, so the shared kernel may belong to that VM rather than the desktop operating system.

An **image** is a packaged template, typically built from layers. A **container** is an instantiated workload created from an image, usually with a writable layer and runtime configuration. A **registry** stores and distributes images. A **runtime** prepares and executes containers.

```text
Source code + dependencies + build instructions
                       |
                    Build
                       |
                  Container image
                       |
                     Registry
                       |
                Pull and instantiate
                       |
                Running container
```

Image layers can be reused between builds. This improves deployment efficiency but also means a vulnerable base image can propagate to many services. A tag such as `latest` is a mutable reference; a digest identifies specific image content. Pinning a digest improves reproducibility, but it does not establish that the content is trustworthy or free of vulnerabilities.

### How container isolation works

Linux namespaces restrict what processes can see, such as process identifiers, network interfaces and mount points. Control groups (cgroups) account for and limit resource consumption, including memory and CPU. Linux capabilities divide traditionally privileged operations into smaller permissions. Seccomp can restrict system calls; mandatory access control systems such as SELinux and AppArmor can impose additional policy restrictions.

| Mechanism | Purpose | Example |
| --- | --- | --- |
| Namespace | Restrict process view of resources | Separate network interfaces |
| Cgroup | Account for and limit resources | Cap memory consumption |
| Capability | Limit privileged operations | Remove unnecessary network administration privileges |
| Seccomp | Filter system calls | Restrict calls unnecessary for the workload |
| Mandatory access control | Enforce access policy | Restrict filesystem access |

A process that is root inside a container may have different privileges from root on the host, depending on user namespaces and runtime configuration. Nevertheless, unnecessary root execution increases risk. Privileged containers, host filesystem mounts and access to the container management socket can substantially weaken isolation.

**Important limitation:** Containers share a kernel within their execution environment. A kernel vulnerability can threaten multiple containers. A container boundary should not automatically be treated as equivalent to a VM boundary for hostile multitenant workloads.

## 5. Securing the container supply chain and runtime

Container security begins before deployment. Consider a team building an application from an unmaintained base image. Even if its own code is secure, inherited libraries may contain known vulnerabilities. A compromised build pipeline may introduce malicious code into an otherwise legitimate image.

A secure lifecycle includes:

1. Select minimal, maintained base images from trusted sources.
2. Inventory dependencies using a software bill of materials (SBOM).
3. Scan images and dependencies for known vulnerabilities and prioritize remediation based on exposure and exploitability.
4. Sign approved images and verify signatures and provenance at deployment.
5. Restrict registry write access and separate build permissions from production deployment permissions.
6. Run workloads with least privilege, resource limits and restricted network access.
7. Monitor runtime behavior, rebuild patched images and replace outdated instances.

A signature provides evidence about integrity and signing identity under the configured trust policy. It does not prove that signed software is safe. A scanner can identify known vulnerable components but cannot reliably detect every malicious behavior, unknown vulnerability or application logic flaw.

### Runtime configuration

A read-only root filesystem can reduce unauthorized changes, although applications may require designated writable volumes. Secrets should be retrieved through an approved secret management mechanism rather than embedded in images or committed to source control. Deleting a secret in a later image layer may leave it recoverable from earlier layers; exposed credentials should be rotated.

Container management interfaces require strong authentication and authorization. Mounting a powerful runtime socket into an application can give it control over other workloads or the host, depending on the runtime configuration. Avoid this unless explicitly required and carefully isolated.

### Safe local inspection

If Docker is already installed and you have permission to inspect your own local environment, these read-only commands illustrate the distinction between images and containers:

```bash
docker image ls
docker ps -a
```

The first lists local images, including tags and image IDs. The second lists running and stopped containers; `-a` means all containers. An empty list is a valid result. No installation, image download or running container is required for this lesson.

For an existing authorized container, replace `CONTAINER_NAME` with its actual name:

```bash
docker inspect --format '{{.HostConfig.Privileged}}' CONTAINER_NAME
```

The format expression selects the privileged-mode setting rather than printing the entire configuration, which may contain sensitive values. A result of `false` means privileged mode is disabled, not that every other permission is safe. A result of `true` warrants reviewing the workload's justification and isolation requirements.

## 6. Orchestration and microservices

Container orchestration automates deployment, scheduling, scaling and recovery. Kubernetes is one example. A Kubernetes **pod** is a deployable unit containing one or more containers that share a network namespace and may share volumes. A cluster contains worker nodes and a control plane that manages desired state.

A microservices architecture divides application functionality into independently deployable services. Containers are a common implementation choice, but microservices do not require containers, and a containerized application need not use microservices.

A request to a shopping application might travel from an ingress component to an order service and then to an inventory service. Each communication path is a potential trust boundary. Authentication at the public gateway does not automatically authorize every internal service call.

| Concern | Control |
| --- | --- |
| Excessive cluster administration privileges | Role-based access control (RBAC), separate administrative identities |
| Unrestricted service-to-service traffic | Network policies, authenticated service identities |
| Unapproved images | Admission policies and image verification |
| Workload resource exhaustion | Requests, limits and quotas |
| Exposed secrets | Restricted secret access and appropriate encryption |
| Compromised workload | Runtime monitoring, isolation and replacement from trusted images |

Kubernetes namespaces organize resources and support policy scoping, but are not by themselves strong security boundaries. Network policy enforcement also depends on the networking implementation. Verify that intended policies are actually enforced.

**Exam tip:** Microsegmentation reduces lateral movement by restricting permitted communication. It complements workload authentication and least privilege rather than replacing them.

## 7. Serverless computing and event-driven execution

Serverless computing allows customers to deploy functionality without directly provisioning and maintaining the underlying servers. Function as a Service (FaaS) is a common serverless model. A function might execute when an HTTP request arrives, a message enters a queue or an object is uploaded to storage.

```text
Event source -> Trigger -> Provider-managed execution environment
                                      |
                               Function code
                                      |
                          Authorized downstream resource
```

The provider manages execution infrastructure and typically handles scheduling, scaling and underlying platform maintenance. Customers remain responsible for code, dependencies, permissions, data handling and service configuration according to the specific service agreement.

A function that processes uploaded invoices might need permission to read from one storage location and write results to another. Giving it unrestricted access to every storage bucket increases the consequences of a code vulnerability or compromised execution identity.

### Security concerns

- **Untrusted events:** Validate event contents, authenticate sources where appropriate and treat input as untrusted even when delivered by a managed service.
- **Overprivileged execution roles:** Assign permissions to the specific function and resources required rather than sharing a broad administrator role.
- **Dependency vulnerabilities:** Maintain and scan application dependencies even when the provider patches the underlying platform.
- **Secrets:** Use managed secret retrieval and restrict access to the function's execution identity.
- **Availability and cost:** Configure rate limits, concurrency controls, quotas and alerts. Automatic scaling can amplify unexpected costs or downstream load.
- **Observability:** Collect invocation logs, errors and audit events without recording unnecessary sensitive payloads.
- **Repeated events:** Design operations to be idempotent where appropriate, meaning repeated processing does not produce unintended duplicate effects.

Execution environments may be reused between invocations. Do not assume temporary files or in-memory state are always cleared between requests; follow the provider's documented isolation and lifecycle behavior.

**Exam trap:** Serverless does not mean there are no servers. It means the customer does not directly manage the underlying server infrastructure for that service.

## 8. Comparing architectures and responsibility boundaries

Day 38 introduced shared responsibility. Apply that principle to the execution platform: identify the component, the party controlling it and the evidence that the control is operating.

| Component | Self-managed VM on IaaS | Self-managed containers on IaaS | Managed FaaS |
| --- | --- | --- | --- |
| Physical infrastructure | Provider | Provider | Provider |
| Hypervisor | Provider | Provider | Provider |
| Guest OS / host kernel | Customer | Customer | Provider-managed execution platform |
| Container runtime | Not applicable unless installed | Customer | Provider if used internally |
| Application dependencies | Customer | Customer | Customer for supplied dependencies |
| Workload permissions | Customer | Customer | Customer |
| Application data | Customer governance | Customer governance | Customer governance |

This is an illustrative allocation, not a universal contract. Managed Kubernetes, managed container services and custom serverless runtimes change the division of responsibility. Providers also have obligations to protect data within their service boundaries.

### Choosing an architecture

| Requirement | Possible fit | Trade-off |
| --- | --- | --- |
| Different guest operating systems | VMs | More OS maintenance and resource overhead |
| Repeatable application deployment | Containers | Shared kernel and image lifecycle risks |
| Short event-driven processing | Serverless functions | Runtime limits and provider dependency |
| Stronger separation of untrusted workloads | Separate VMs or dedicated hosts, as appropriate | Higher cost and management complexity |
| Long-running stateful service | VM or container with managed persistent storage | Recovery and storage consistency require planning |

No architecture is inherently the most secure. A poorly configured VM may be less secure than a well-managed container platform. Select the boundary appropriate to the threat model, operational capacity, compliance requirements and failure consequences.

🧠 **Memory hook:** Follow the kernel, follow the identity, follow the data. These three questions reveal much of the architecture's security responsibility.

## 9. Worked case study: A university document processing platform

A university accepts scholarship applications through a public portal. It needs a web application, a document processing service and a database. Documents contain personal information. The team proposes a containerized portal and a serverless document processing function.

The initial design places the web application and database on an unrestricted shared network. Both use the same administrative credential. Uploaded files trigger a function with full storage access. Images are pulled using mutable tags from an external registry.

The security team maps the actual data flows before choosing controls. The portal needs to accept HTTPS requests and submit application records. It does not need database administration privileges. The processing function needs access only to newly uploaded documents and its designated output location. The database should not accept connections from the public internet.

| Finding | Risk | Design change | Residual concern |
| --- | --- | --- | --- |
| Shared administrative credential | One compromise affects multiple services | Separate least-privilege workload identities | Identity policies require periodic review |
| Unrestricted internal network | Lateral movement | Explicit allowed service flows | Allowed flows can still carry malicious requests |
| Mutable external image tags | Unreviewed code changes | Approved registry, digest pinning and signature verification | Approved images may still contain vulnerabilities |
| Broad function storage access | Exposure of unrelated documents | Restrict read/write permissions to required locations | Malicious input may exploit processing code |
| No resource limits | Denial of service or cost increase | Quotas, concurrency limits and monitoring | Legitimate peak demand may be throttled |

The university stores application records outside ephemeral execution environments, maintains independent backups and tests restoration. It logs access decisions and processing failures without logging full scholarship documents. The team also tests whether unauthorized service-to-service requests are actually denied.

**Takeaway:** Architecture diagrams become useful security artifacts when they identify identities, data flows, enforcement points and shared failure dependencies rather than merely listing technologies.

## 10. Guided notes — Write first, reveal later

| Prompt | Your answer |
| --- | --- |
| What distinguishes a VM from a container? | _Write here._ |
| How do Type 1 and Type 2 hypervisors differ? | _Write here._ |
| What is a VM escape? | _Write here._ |
| Distinguish an image, registry and running container. | _Write here._ |
| Compare namespaces and cgroups. | _Write here._ |
| Why are image scanning and signature verification complementary? | _Write here._ |
| What responsibilities remain with a serverless customer? | _Write here._ |
| Why should service identities and network permissions both be restricted? | _Write here._ |

<details markdown="1">
<summary>Reveal the completed notes table (sample answers)</summary>

| Prompt | Model answer |
| --- | --- |
| VM versus container | A VM runs a guest OS on virtual hardware; a conventional container isolates processes sharing an execution environment's kernel. |
| Hypervisor types | Type 1 runs directly on hardware in the conventional model; Type 2 depends on a host OS. |
| VM escape | A breach of guest isolation that reaches the host or hypervisor environment. |
| Image, registry, container | A packaged template, a distribution/storage service, and an instantiated workload respectively. |
| Namespaces and cgroups | Namespaces restrict resource visibility; cgroups account for and constrain resource usage. |
| Scanning and signing | Scanning identifies known risks; signatures support integrity and provenance verification. Neither proves safety. |
| Serverless responsibility | Code, supplied dependencies, identities, permissions, data and configuration remain customer responsibilities under the service agreement. |
| Identity and network controls | Network policy limits reachable destinations; identity authorization limits permitted operations even on reachable services. |

</details>

## 11. Hands-on lab: Diagram a containerized service

**Time:** 20–25 minutes  
**Tools:** Paper and pencil or any local text editor. Optional diagramming software.  
**Safety and scope:** All systems and data are fictional. No cloud account, network scanning or deployment is required.

### Scenario

A small online bookstore runs a public reverse proxy, a catalog API container, an order API container and a database container on a single Linux VM. A managed serverless function sends order confirmation messages after receiving queue events. The database uses a persistent volume. The bookstore has one approved image registry.

The initial configuration allows every container to communicate with every other container. The database is exposed publicly. Both APIs use the same database administrator credential. The order function can read all storage resources. Images use mutable tags and no resource limits are configured.

### Step 1 — Draw the execution boundaries (5 minutes)

Draw the physical/cloud infrastructure, the VM boundary, its Linux kernel and container runtime. Place the four containers inside the VM. Draw the persistent volume attached to the database. Place the registry, queue and managed function outside the VM boundary. Label the provider-managed function environment separately.

### Step 2 — Map intended data flows (5 minutes)

Draw arrows for these legitimate operations:

- Internet clients send HTTPS requests to the reverse proxy.
- The proxy forwards catalog and order requests to their respective APIs.
- The catalog API reads product records; the order API writes order records.
- The order API publishes confirmation events to the queue.
- The managed function consumes queue events and sends confirmations through an approved messaging service.
- The deployment system pulls approved images from the registry.

Label arrows with purpose and identity. Do not draw a public connection directly to the database.

### Step 3 — Assign controls (5 minutes)

Complete the following table independently.

| Risk | Proposed control | Responsible party |
| --- | --- | --- |
| Public database | _Write here._ | _Write here._ |
| Shared database administrator credential | _Write here._ | _Write here._ |
| Unrestricted container traffic | _Write here._ | _Write here._ |
| Mutable image tags | _Write here._ | _Write here._ |
| Excessive function permissions | _Write here._ | _Write here._ |
| Resource exhaustion | _Write here._ | _Write here._ |
| Loss of database container | _Write here._ | _Write here._ |

### Step 4 — Interpret your design (5–10 minutes)

1. Which workloads share a kernel, and what happens if that kernel is compromised?
2. Does removing public database exposure prevent every database attack?
3. Why should catalog and order services have separate database identities?
4. Does moving confirmation processing to serverless eliminate dependency patching?
5. What should happen if a database container is replaced? What if its persistent volume is lost?
6. Identify one remaining single point of failure and propose a mitigation.

<details markdown="1">
<summary>Reveal lab answers</summary>

A possible logical diagram is:

```text
Internet --HTTPS--> [Proxy]
                     |   |
                [Catalog] [Order] ---> Queue ---> Managed function
                     |       |                         |
                     +--[Database]                 Messaging service
                            |
                      Persistent volume

Proxy, Catalog, Order and Database share the Linux VM kernel.
Approved registry --> authorized deployment system --> runtime
```

The diagram is simplified: arrows represent intended flows, not proof that enforcement exists. The VM boundary encloses the four containers and their shared runtime/kernel; the queue and function are separate managed services in this scenario.

| Risk | Proposed control | Responsible party |
| --- | --- | --- |
| Public database | Remove public listener/exposure; allow only authorized API traffic | Bookstore platform team |
| Shared administrator credential | Separate read-only catalog and scoped order identities | Bookstore database/application team |
| Unrestricted traffic | Default-deny policy with explicit required flows | Bookstore platform team |
| Mutable tags | Approved registry, verified signatures and pinned digests | Bookstore build/deployment team |
| Excessive function permissions | Scope role to queue consumption and messaging operations | Bookstore cloud/application team |
| Resource exhaustion | Container limits, function concurrency controls and alerts | Bookstore; provider maintains managed infrastructure |
| Loss of database container | Persistent storage plus independent tested backups | Bookstore, with provider storage obligations as contracted |

1. All four containers share the VM's Linux kernel. Kernel compromise may undermine their isolation; patching and stronger workload separation can reduce risk.
2. No. An authorized API can be compromised, credentials stolen or malicious queries submitted through permitted paths.
3. Separate identities constrain the consequences of compromise and improve accountability. The catalog should not acquire order-writing or administrative permissions unnecessarily.
4. No. The customer still maintains supplied application dependencies and code.
5. A replacement container should reconnect to its persistent data if the volume survives. If the volume is lost, recovery requires an independent backup or other recovery mechanism.
6. The single VM is a shared failure domain. Redundant hosts and a tested failover design can improve availability, provided storage and networking dependencies are also addressed.

**Expected observation:** Your revised diagram should contain fewer permitted communication paths, distinct identities and explicit shared dependencies. A diagram alone does not validate enforcement; production testing would be required.

</details>

## 12. Eight original Security+-style questions

These are original practice questions, not actual CompTIA exam items.

### Q1. Which characteristic BEST distinguishes conventional Linux containers from virtual machines?

A. Containers always have dedicated physical processors.  
B. Containers share an execution environment's kernel.  
C. Virtual machines cannot run multiple applications.  
D. Virtual machines do not require operating systems.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Containers ordinarily share a kernel, whereas conventional VMs run guest operating systems. A is not required; C and D misrepresent VM operation.

</details>

### Q2. An attacker exploits a vulnerability to cross from a guest VM into the hypervisor environment. What has occurred?

A. VM sprawl  
B. Resource reservation  
C. Image drift  
D. VM escape

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** VM escape breaches the guest isolation boundary. Sprawl concerns unmanaged proliferation, reservation concerns resource allocation, and image drift concerns differences in deployed image state or versions.

</details>

### Q3. Which mechanism is MOST directly responsible for limiting a Linux container's memory consumption?

A. Cgroups  
B. Image signatures  
C. Process namespaces  
D. Registry authentication

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** Cgroups account for and constrain resources. Namespaces isolate views of resources, while signatures and registry authentication address software integrity and access.

</details>

### Q4. A deployment team verifies the signature of an approved container image. What can it reasonably conclude?

A. The image cannot contain vulnerable libraries.  
B. The image cannot execute malicious code.  
C. The image satisfies the configured signature verification policy.  
D. The image no longer requires runtime restrictions.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Verification establishes evidence of integrity and signing identity under a trust policy. It does not establish absence of vulnerabilities or malicious behavior, nor replace runtime controls.

</details>

### Q5. A serverless function reads files from one designated storage location. Which control BEST limits the impact of compromised function code?

A. Grant the function the cloud account administrator role.  
B. Restrict its execution role to required operations on that location.  
C. Increase the function's memory allocation.  
D. Disable invocation logging.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**B.** Least privilege constrains what compromised code can access. A expands impact, C addresses capacity, and D reduces visibility without limiting access.

</details>

### Q6. Which action BEST reduces lateral movement between containerized microservices?

A. Give every service the same privileged identity.  
B. Store all credentials in a shared image.  
C. Use a mutable image tag for all deployments.  
D. Enforce explicit service-to-service network policies and scoped identities.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**D.** Network policies restrict reachability and scoped identities restrict authorized operations. A and B increase shared compromise risk; C does not restrict network access.

</details>

### Q7. A company migrates application code to a managed FaaS platform. Which responsibility normally remains with the company?

A. Remediating vulnerabilities in its supplied application dependencies.  
B. Replacing failed physical server processors.  
C. Patching the provider's underlying hypervisor.  
D. Maintaining the provider's physical data center access controls.

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**A.** The customer remains responsible for supplied code and dependencies. B, C and D normally belong to the provider in a managed FaaS model; consult the actual service agreement.

</details>

### Q8. A database container is replaced successfully, but its storage volume has been destroyed. Which preparation MOST directly supports data recovery?

A. A signed application image  
B. A larger CPU quota  
C. Independent, tested database backups  
D. A separate process namespace

<details markdown="1">
<summary>Reveal answer and explanation</summary>

**C.** Independent tested backups support recovery when persistent storage is lost. Images package software rather than necessarily containing current database records. CPU quotas and namespaces do not restore data.

</details>

**Score guide:** 7–8 correct: strong understanding; 5–6: revisit Sections 3–8 according to missed questions; 0–4: repeat Sections 10–11 and retry.

## 13. Discussion challenge

A legal aid organization wants to host confidential client records and run unpredictable bursts of document conversion. Its small IT team can maintain a few VMs but has little experience operating container orchestration. A vendor recommends migrating everything to serverless services; another recommends a self-managed Kubernetes cluster.

Propose an architecture that balances isolation, operational capacity, cost, confidentiality and recovery. Explain which workloads you would separate, who would maintain each platform component, how you would restrict workload identities and how you would verify that sensitive data is not exposed through logs or temporary storage. Identify at least two trade-offs and one risk your design cannot completely eliminate.

**Bonus:** Explain how a shared infrastructure failure could affect the CIA triad even when all workload identities are correctly configured.

## 14. Study from the Treadmill 🏃

**Direct links to specific videos:** Editorial verification pending. Locate the Professor Messer SY0-701 lessons on architecture models and virtualization/containerization (Objective 3.1). Exact video URLs have not been verified and must be added before publication.

**Active-listening prompts:**

- Which component enforces isolation in each execution model?
- Which responsibilities move to the provider when adopting serverless computing, and which remain with the customer?
- How do shared components affect the blast radius of a compromise?

## 15. One-minute recap

- **Virtual machines** run guest operating systems on virtual hardware mediated by a hypervisor.
- **Containers** isolate processes while sharing an execution environment's kernel.
- **Namespaces** restrict visibility; **cgroups** constrain resource usage.
- **Image scanning and signing** address different supply-chain risks; neither guarantees safety.
- **Orchestration** automates deployment but introduces management and service communication boundaries.
- **Serverless** abstracts infrastructure management, not application security responsibilities.
- **Golden rule:** Identify the shared component, the workload identity and the data boundary before selecting a security control.

**Next:** Day 40 — Title pending curriculum confirmation.

---
© 2026 Security+ Study Academy | Created by [ghostparticle404](https://github.com/ghostparticle404).
All rights reserved. This original educational content is provided free of charge for personal, noncommercial study. Reproduction, redistribution, modification for republication, or commercial use requires prior written permission.

Security+ is a trademark of CompTIA, Inc. This independent educational resource is not affiliated with or endorsed by CompTIA.
