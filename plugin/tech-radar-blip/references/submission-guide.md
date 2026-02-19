# Tech Radar Blip Submission Guide

## Ring Definitions and Criteria

### Adopt

**Definition:** Proven and mature. The industry should embrace this.

**Criteria for Adopt:**
- Multiple successful production deployments across different contexts
- Well-understood benefits and trade-offs
- Mature ecosystem with good documentation and community support
- Low risk for most organizations to adopt
- Clear evidence of long-term viability

**Evidence required:**
- Broad production usage across multiple clients/projects
- Track record of stability and reliability
- Industry recognition and adoption trends

### Trial

**Definition:** Worth pursuing. Used at least once in production.

**Criteria for Trial:**
- Demonstrated value in at least one production environment
- Promising results that warrant broader experimentation
- Acceptable risk profile for production use
- Sufficient maturity for teams to try with reasonable confidence

**Evidence required:**
- At least one production deployment (mandatory)
- Concrete outcomes and learnings from production use
- Clear understanding of when and where it applies

### Assess

**Definition:** Worth exploring. Interesting to watch.

**Criteria for Assess:**
- Shows promise and potential value
- May not yet have production validation
- Worth investing time to understand and evaluate
- Could become important in the near future

**Evidence required:**
- Clear articulation of potential value
- Understanding of the problem space it addresses
- Awareness of current limitations or unknowns

### Caution (formerly Hold)

**Definition:** Proceed with care. The industry should avoid using this.

**Criteria for Caution:**
- Known problems, risks, or limitations
- Better alternatives typically exist
- May have been previously recommended but circumstances changed
- Using it introduces unnecessary risk or technical debt

**Evidence required:**
- Specific concerns or problems encountered
- Explanation of why alternatives are preferred
- Context for when caution is warranted

## Quadrant Definitions

### Tools

Software tools that support the development process, operations, or day-to-day work.

**Examples:** IDEs, CI/CD tools, monitoring solutions, testing frameworks, build tools, code analysis tools

**Ask:** Is this something developers or operators use as a tool in their workflow?

### Techniques

Ways of working, methodologies, approaches, or practices.

**Examples:** Architectural patterns, testing approaches, development practices, organizational patterns, design methods

**Ask:** Is this a way of doing something rather than a specific tool or technology?

### Languages & Frameworks

Programming languages, development frameworks, and libraries that form the foundation of applications.

**Examples:** Programming languages, web frameworks, mobile frameworks, data processing frameworks, major libraries

**Ask:** Is this something you write code in or build applications on top of?

### Platforms

Infrastructure, cloud services, databases, and foundational systems that applications run on.

**Examples:** Cloud providers, container orchestration, databases, message queues, operating systems, serverless platforms

**Ask:** Is this something applications are deployed to or run on?

## Writing Strong Submissions

### The "Why Now" Section

This is often the most important part of a submission. Address:

- What has changed recently that makes this relevant?
- Is there a trend or shift in the industry?
- Has the technology reached a maturity milestone?
- Are there new use cases emerging?
- Has adoption reached a tipping point?

**Weak example:** "Kubernetes is popular and many companies use it."

**Strong example:** "With the maturation of managed Kubernetes offerings (EKS, GKE, AKS) and the emergence of GitOps practices, Kubernetes has become accessible to organizations without dedicated platform teams. The ecosystem of operators and CRDs has also reached a point where common patterns are well-established."

### The Description Section

Write for someone who has never encountered this technology:

- What problem does it solve?
- What category does it fall into?
- How does it differ from alternatives?
- What is its primary use case?

**Weak example:** "A tool for managing infrastructure."

**Strong example:** "Pulumi is an infrastructure-as-code tool that allows teams to define cloud infrastructure using general-purpose programming languages (TypeScript, Python, Go, C#) rather than domain-specific languages. This enables using familiar testing frameworks, IDE features, and code sharing practices for infrastructure code."

### The Value Proposition

Be specific about concrete benefits:

- Quantifiable improvements (speed, cost, reliability)
- Developer experience improvements
- Risk reduction
- Capability enablement

**Weak example:** "It makes development faster."

**Strong example:** "Teams using this approach reported 40% reduction in deployment failures and were able to ship features to production within hours rather than days, primarily due to the confidence provided by comprehensive automated testing."

## Example: Strong Submission

```
BLIP SUBMISSION
===============

Proposed Blip Name: Platform Engineering Teams

Quadrant: Techniques
Ring: Trial

Description:
Platform Engineering Teams are dedicated groups focused on building and maintaining internal developer platforms (IDPs) that abstract infrastructure complexity and provide self-service capabilities to product development teams. Unlike traditional DevOps or SRE roles that often embed within product teams, platform engineering creates centralized teams that treat the platform as a product, with product development teams as their customers.

Why Blip This Now?
The complexity of cloud-native development has reached a point where expecting every development team to manage their own infrastructure, CI/CD, and observability creates significant cognitive load and inconsistency. Organizations are recognizing that centralizing platform concerns (while enabling self-service) improves both developer productivity and operational consistency. The rise of tools like Backstage, Crossplane, and internal developer portals has made platform engineering more practical to implement.

Value Proposition:
Organizations implementing platform engineering have seen reduced onboarding time for new developers (from weeks to days), decreased infrastructure-related incidents through standardization, and improved developer satisfaction by reducing toil. Product teams can focus on business logic rather than infrastructure concerns, while still maintaining autonomy through self-service capabilities.

Production Experience:
- Used in production: Yes
- Client(s): Financial services client (2 years), Healthcare technology company (18 months)
- Recommending team/community: Cloud Practice, Developer Experience Community

Vertical/Market Relevance: General, though particularly valuable in regulated industries where standardization aids compliance

Reference URLs:
- https://platformengineering.org/
- https://www.gartner.com/en/articles/what-is-platform-engineering
- https://backstage.io/

Existing Blip URL: N/A (new submission)
```

## Submission Checklist

Before submitting, verify:

1. **Completeness**
   - All required fields are filled
   - Description is clear to newcomers
   - "Why now" explains current relevance
   - Value proposition is concrete and specific

2. **Evidence**
   - Ring selection is supported by evidence
   - Production experience is documented (if claiming Trial+)
   - Reference URLs are provided and current
   - Recommending team is identified

3. **Defensibility**
   - Could you explain this in a 5-minute discussion?
   - Are potential objections addressed?
   - Is the quadrant choice clearly justified?
   - Is the ring appropriate for the evidence provided?

4. **Quality**
   - Writing is clear and concise
   - Jargon is explained or avoided
   - Claims are substantiated
   - Tone is objective and professional
