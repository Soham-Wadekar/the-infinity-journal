# ⚙️ Jenkins

> _"Build, test, and deploy — continuously."_ — Jenkins Philosophy

Jenkins is an open-source automation server that enables continuous integration (CI) and continuous delivery (CD) for software projects.  
It allows teams to automate the process of building, testing, and deploying applications, ensuring faster releases, improved code quality, and seamless collaboration.

With its vast plugin ecosystem and flexible architecture, Jenkins supports diverse tech stacks, integrates with countless tools, and adapts to projects of any size — from small teams to enterprise-scale DevOps workflows.

## 📚 Contents

- [Jenkins Architecture](#architecture)
<!-- - [Terminology](#terminology)
- [Basic Commands & Operations](#basic-commands--operations)
- [Jobs & Pipelines](#jobs--pipelines)
- [Plugins & Integrations](#plugins--integrations)
- [Security & User Management](#security--user-management)
- [Scaling Jenkins](#scaling-jenkins) -->

By automating repetitive tasks and enabling reliable CI/CD pipelines, Jenkins streamlines software delivery, reduces manual errors, and empowers teams to release high-quality software more frequently.

---

## Architecture

Jenkins follows a **Master-Agent** model

The Master (controller) handles the UI, job scheduling, plugin management, secdurity and decides what needs to be done.

The Agent (Worker Node) executes jobs. It can be present on the same machine or on remote servers. There are two types of agents - Permanent and Cloud agents. Permanent agents are always connected to Jenkins, will run indefinitely unless manually stopped and can be connected through SSH. Cloud agents are temporary agents that are connected to Jenkins when a job starts and are deleted when the job finishes; they are managed by a plugin (Docker, Kubernetes, AWS Fleet Manager, etc.)
