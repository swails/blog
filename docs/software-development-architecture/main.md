# The Anatomy of a Successful Software Engineering Capability

There are more ways to engineer software poorly than well. The better a software engineering capability is run, the more quantifiable value is added by that capability. More efficient software development practices yield more features over a given period of time, fewer bugs, and a faster mean time to resolve issues in production (bugs and issues are faster to identify, isolate, reproduce, fix, and deploy or release updates). This can either drive additional revenue or boost efficiency to reduce operating expenses.

The purpose of this article is to describe the minimal tooling needed to build the foundation of successful software engineering operations. I will not discuss best practices in coding, but rather how to deploy and configure code development tools in a way that provides a stable foundation for the growth of a healthy and productive software development capability from inception.

## What does it mean to have sustainable software development?

A sustainable software engineering capability is one that is able to continuously deliver added value at a consistent pace. Unfortunately, identifying unsustainable practices may happen long after said practices are already adopted.

The early stages of development of new software are easy to show rapid progress. A *new* software project starts with no features, no bugs, no users, and very frequently few developers. Under such ideal conditions, churning out new code and features is fast, fun, and exciting. The software remains fairly simple, and therefore easily conceptualized in its entirety by the small team of developers that worked to start it.

As a successful project matures its number of features and bugs grows alongside the size of its user base and development team. It is at this stage that many poorly-run software engineering functions encounter problems. Poor software engineering practices can make teamwork challenging and inefficient, leading to a sharp decline in the pace of delivering features and fixing (or even identifying) bugs.

A sustainable software engineering function does not suffer from these inefficiencies. They can onboard new team members and enable them to start contributing quickly. Identifying and fixing bugs is streamlined as much as possible, resulting in software that is more robust and reliable.

## Process and Sustainable Software Development

I've found one of the key differentiators between well-run and poorly-run software projects is the effective use of [process](https://en.wikipedia.org/wiki/Software_development_process) and automation.

*Process* is the combination of policy and prescribed operational steps required for any software development or release activity. For example, requiring all changes to pass through code review and be approved by at least one "code owner" before being merged is an example of process in code development. Requiring that released software be given a "tag" with the version number which is subsequently built into the source code before deploying a package is an example of process in software release or deployment.

Choosing the right software development tools makes it possible to *enforce* policy and process by explicitly prohibiting either intentional or accidental violations. For example, a proper version control system (VCS) will allow you to "protect" branches by prohibiting changes made without pull requests or lacking specific approvals following code review. Likewise, a suitably configured CI/CD service and package manager repositories will only permit package deployment that passes through an automated process that performs the necessary version injection and build steps from a consistent environment like a specific Docker container image.

Teams that opt to forego implementing process inevitably become paralyzed by that choice as both the team and code base grows. Identifying and reproducing bugs that appear in production becomes a chore as significant time needs to be spent determining what *exact* version of the code was deployed, *how* it was built and/or deployed, and reproducing the issue in a way that it can be reliably fixed. The complexity of deploying software grows as new features require small tweaks which continue to build up. Fear of breaking the house of cards drives teams to lengthen the deployment schedule and increase the effort expended each time.

Software developers rightly recognize this as a description of [technical debt](https://en.wikipedia.org/wiki/Technical_debt) – and that's exactly what this is. Savvy use of process can help limit the accumulation of operational technical debt – specifically the mechanics of collaborating on code development and releasing or deploying software products.

# Designing Sustainable Software Engineering

For startups or other small companies that are adding a software engineering capability for the first time, the small core team of software developers may lack the experience needed to establish sound practices at the outset. The rest of this article is devoted to what tooling is needed, and how it should be configured and employed to establish your software engineering capability on a sound footing.

I've found that a sufficient nucleus that helps foster a robust and sustainable software engineering capability can be deployed via the components shown below.

![Software Engineering Core](tooling.svg)

In the sections below, I'll briefly describe the importance of each component as well as the options and recommendations to satisfy each one.

## Auth Provider

The Auth Provider supplies both *Authentication* (establishing that a user or application *is* who they claim to be) and *Authorization* (declares that the authenticated user has the permission to do something).

Having a centralized Auth Provider is critical to scaling an organization. While a company may start out by employing a small handful of services, those few services quickly grows. Without a centralized Auth Provider, each application will need to maintain its own internal list of users making it effectively impossible to implement security standards surrounding multifactor authentication and strength of password.

*Every* tool you onboard into your organization should integrate using some standardized Auth mechanism, like SAML 2.0, to your centralized Auth Provider. This is a feature known as "Single Sign-On", or SSO – that is you are able to sign into every company service or system with a single account. I would go so far as to exclude any tool that does not offer such integration from consideration altogether.

### Recommendation

You typically have little choice in this Auth Provider, as it is usually established with the company. Common choices are Azure Active Directory (from Microsoft), or an offering from Okta or Auth0. Any of these will support SAML 2.0 and enable SSO.

## Version Control System (VCS)

Building modern, collaborative software projects is impossible without a Version Control System. The most common commercial options for these platforms are [GitHub](https://github.com), [GitLab](https://gitlab.com), and [BitBucket](https://bitbucket.org). There are, of course, many others.

The VCS will serve as the centralized hub that drives almost all of your software engineering process. It is here that you will define automations of your developer operations, code review policy, and tag versioned releases of your source code.

### Recommendation

I've used all 3 in professional settings. I've managed a private GitLab instance for a community-driven project and used (and administered) both GitHub and BitBucket Server at commercial companies.

I would personally recommend GitHub or GitLab, as the integrated CI/CD offering provided with both the public and hosted versions of the platform allows you to leverage the same platform for for both VCS and CI/CD.

## CI/CD Service

The CI/CD service – standing for Continuous Integration and Continuous Deployment (or Continuous Delivery) – is the platform that allows you to automate developer operations. You will use it to automatically run a test suite to gate pull requests to drive code quality, and it provides a platform to automate building and deploying software packages.

### Recommendation

If you selected either GitHub or GitLab as the VCS provider, I highly recommend using either GitHub Actions or GitLab CI, respectively. Both are mature, capable platforms that will allow you to do almost anything you want.

If GitHub Actions or GitLab CI are not options or you are using a self-hosted GitLab instance without access to their CI infrastructure, I would recommend a platform that provides its own hosted infrastructure that runs builds like Azure Pipelines or CircleCI. Other options, like Jenkins or BuildKite, require you to supply your own machines (which you must keep secure and up-to-date) to execute pipelines.

## Package Manager Repositories

This piece is critical and often overlooked. Package manager repositories provide a place to push software packages that can then be deployed to production services or pulled by developers (or customers) to deploy directly to their devices. Most programming languages come with package managers built-in, summarized for some languages below.

| Language | Package Manager | Public Package Repository |
|----------|-------|----|
| Python | pip | pypi.org |
| Python | conda | conda.anaconda.org |
| JavaScript | npm | registry.npmjs.org |
| C# | nuget | nuget.org |
| Docker | docker | hub.docker.co | 
| Rust | cargo | crate.io |
| swift | Swift Package Manager | swift.org |
| Kubernetes | helm | Decentralized |

Even if your organization only uses one, or predominantly one, programming language, you are likely to use multiple package managers for different technologies in your stack. For instance, Docker container images are often leveraged to provide consistent, reliable environments for running continuous integration testing and to build software packages you intend to publish for release.

Publishing your libraries to package manager repositories makes it easy to deploy well-defined, reproducible production environments to any computer infrastructure in your organization. This includes developer machines, a private data center, or either public or private clouds. Environments can often be declared in a single file that lists the packages with any relevant version pins.

While tags in your VCS can often be modified, you can establish policy in your package repositories to prohibit overwriting or deleting packages. This eliminates uncertainty surrounding the *exact* version of the code deployed in production that encountered a reported bug relative to a strategy that deploys software versions through git tags.

### Recommendation

Most public package manager repositories require paid service contracts to provide private package repositories (which you then must trust them to protect). Not all of these may support SSO. There are also platforms that provide interfaces for many different package managers, and I highly recommend using one.

Both [GitHub](https://github.com/features/packages) and [GitLab](https://docs.gitlab.com/ee/user/packages/package_registry/supported_package_managers.html) offer repositories for a small number of package managers. If your VCS is sufficient, use that as your package repository. Dedicated package repository services are extremely valuable and well worth the monthly expense. Both [JFrog Artifactory](https://jfrog.com/artifactory/) and [Sonatype Nexus](https://www.sonatype.com/products/nexus-repository) satisfy the requirements here.

I recommend using Artifactory, as it is the only platform with a fully managed option (Nexus requires you to provision and maintain your own infrastructure to host it).

## Issue Tracker

Issue trackers are used to keep track of work that needs to be done from fixing bugs to adding new features. They help to communicate priorities as the team grows, and when used properly can facilitate efficient teamwork by allowing any of a number of developers to work on a given task.

If and when your organization grows to the point where a project manager is a worthwhile investment, the issue tracker becomes one of their most valuable tools to interface with the engineering team.

### Recommendation

GitHub and GitLab both provide issue trackers as part of a source code repository. While less flexible than dedicated solutions, they can satisfy this role for early-stage development.

When a more comprehensive solution is required, the only issue tracker I've used that I would recommend is Jira. Other popular options I've not tried before include Asana and Trello. There are *many* others.

## Wiki

While developing software, it is important to record important decisions and information about the design of your software. This is important both to help onboard new team members as the team grows and to remind the original authors of important details they may have forgotten.

I've found wikis to be an effective medium to keep this sort of documentation (although using the web hosting abilities of GitHub and GitLab by rendering markdown or reStructuredText to web pages using utilities like `sphinx` or `mkdocs` would also work).

### Recommendation

For basic code-documenting wikis, the offering provided by the VCS (like GitHub or GitLab) may be sufficient. At a minimum, it is a good place to start, and content can be migrated if your needs expand to require a more comprehensive solution.

For a dedicated solution, I would recommend Confluence. It is easy to get started with, but has plenty of advanced features to help standardize and organize content generation.