Project Context — AI Kubernetes Access Manager

I am building an AI-powered Kubernetes Access Manager as a DevOps + AI project.

The goal is to allow an administrator to give natural-language access requests such as:

"Give Rohan read-only access to pods in the dev namespace."

The system should interpret the request, convert it into a structured access policy, generate the required Kubernetes Role and RoleBinding, validate the requested permissions against security rules, optionally require human approval, apply the configuration through the Kubernetes API, and finally verify that the requested permissions were actually granted.

The planned flow is:

User
→ Natural-language request
→ AI/LLM
→ Structured access request
→ Policy validation/security guardrails
→ Human approval
→ Kubernetes API
→ Role + RoleBinding
→ Permission verification
→ Audit log

For example, the user might enter:

"Give Rohan read-only access to pods in dev."

The AI should convert this into:

User: rohan
Namespace: dev
Resource: pods
Permissions: get, list, watch

The application then creates a Kubernetes Role and RoleBinding that give Rohan those permissions.

The AI should not directly execute arbitrary kubectl commands. The AI should only interpret the user's request and produce a structured access request. The application should handle validation, security rules, approval, and execution.

Initial technology stack:

Python
FastAPI
Kubernetes Python client
Kubernetes RBAC
kind for local Kubernetes development/testing

Later, the project can include:

LLM integration
Approval workflow/UI
Audit logging
AWS IAM
EKS Access Entries
EKS + Kubernetes RBAC integration
Docker
CI/CD

Development phases:

Phase 1: Python → Kubernetes API → Role/RoleBinding

Phase 2: Build a FastAPI REST API

Phase 3: Add an LLM to convert natural-language requests into structured access requests

Phase 4: Add security validation and guardrails

Phase 5: Add an approval workflow

Phase 6: Add permission verification and audit logs

Phase 7: Integrate with AWS EKS and IAM

The main objective is to combine Kubernetes RBAC, Kubernetes API automation, Python, AWS/EKS, and AI while maintaining a secure architecture where the AI does not have unrestricted control over the Kubernetes cluster.


-----------------------------------