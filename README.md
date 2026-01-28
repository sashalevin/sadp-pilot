# Supplier ADP (SADP) Pilot

## Summary

This repository documents the Supplier-as-ADP Pilot project. This README and other documentation should be considered works-in-progress and are likely to be incomplete.

## Status

Expected CVE test environment in early February 2026.

## Background

Software is made of other software. When the upstream software has a vulnerability, it'd be nice to know if and to what extent downstream software is affected, and what to do about it. This SADP pilot is a way for the CVE Program to decide if and how to support status information about downstream inheritance of upstream vulnerabilities. While not strictly planning to implement [Vulnerability Exploitability eXchange (VEX)](https://github.com/CVEProject/sadp-pilot/blob/main/README.md), the SADP pilot will essentially implement VEX, or at least meet the material [requirements](https://github.com/SBOM-Community/documents/blob/main/README.md#minimum-requirements-for-vex) of VEX.

Two very work-in-progress documents:

* [Supplier-CNA-as-ADP Pilot Overview](https://docs.google.com/document/d/1JLF5vj_8W7KVeqJNhQl8Fj7PLv-ujbqMLTU6LtDlmvI)
* [Supplier-CNA-as-ADP Pilot](https://docs.google.com/document/d/1u2qKAYo7AYOCDYVv1Le58kv5YpzHBsrwuib7OPY-YyU)

[Slides](https://youtu.be/dBtlUz0kUww?si=-YVCs_omWkLost_4) from the 2025 CVE Program Technical Workshop.

## Test CVE Services SADP Pilot Environment

Participating SADPs will have accounts and credentials for a test environment. The instance will use production data and sync roughly daily. SADPs will only be permitted to [add (PUT) SADP information](https://cveawg.mitre.org/api-docs/#/CVE%20Record/cveAdpUpdateSingle) to existing CVE Records and manage users for their SADP organizations.

We have not yet decided when (or even if) to shift from the test to production environments, but the plan is to shift the pilot to production. Before providing production SADP information, participants MUST demonstrate their capability in the test environment.

## Required SADP Information

The point of SADP is for downstream Supplier CNAs to provide information about their Products with respect to an existing Vulnerability in an upstream Product. As such, the follwing information MUST be provided in an SADP container. Consumers must be able to tell that an ADP container is an *S*ADP container, which means that the SADP information is related the SADP's use of (dependency on) the affected Products in the CNA container.

If an ADP container type (and/or `shortName`) indicates Supplier, then the Products in the ADP container use or depend on the Products in the CNA container and the status information is scoped to whether and to what extent the downstream (SADP) Products are affected by the upstream (CNA) Products.

#### ADP Metadata

`containers.adp[].providerMetadata`

`containers.adp[].providerMetadata.x_adpType` with the value: `supplier`.  This would require a future CVE Record Format change and would support other types of ADP such as `enricher`. Another option is to overload `.containers.adp.providerMetadata.shortName` with a string like `"${shortName}-SADP"`.

#### Product Status

`containers.adp[].affected`

#### Product Status Reference

As an alternative to providing product status within the their container, an SADP MAY instead provide a reference to external product status. The format of this reference has not been determined yet. One simple option is a reference URL with a new type (add example). It may be better, or even necessary, to develop a more robust reference schema, more aligned with this [RFD about assertions](https://github.com/CVEProject/cve-schema/pull/472) (add example).

## Optional SADP Information

An SADP MAY populate any other elements of an ADP container. One goal of the pilot is to determine if any elements cause confusion or conflict with other information in a Record, especially elements of the CNA container. CVSS, for example, [is designed to be reassessed in downstream uses](https://www.first.org/cvss/user-guide#Assessing-Vulnerabilities-in-Software-Libraries-and-Similar2:~:text=When%20assessing%20a%20vulnerability%20in%20a%20given%20implementation%20using%20the%20impacted%20library%2C%20the%20metric%20values%20must%20be%20re%2Dassessed%20for%20that%20specific%20implementation%2E), particularly for Products such as libraries and operating system kernels.

## Participants and Scope

CVE consumers should expect to see SADP containers provided by the pilot participants subject to their defined scopes.

| Participant | Scope | Examples |
| :--- | :--- | :--- |
| HeroDevs | .NET 6, others? | |
| Microsoft | chromium, others? | |
| Red Hat | lots of managed software packages | |

## Questions

We both know some questions in advance, and we expect new questions and experience to arise from running the pilot.

Q1. Can I be and ADP? How do I become an ADP?

Q2. There really is a lot of software reuse and dependency. How will CVE manage all that information, technically and procedurally?

Q3. The pilot was a failure, but we added content to the production CVE corpus. How do we roll back?

Q4. A big assumption behind VEX and SADP is that consumers of (CVE) dependency-related vulnerability information can and will use that information to improve vulnerability scanning results. For example, in downstream software, a scanner detects a version of an upstream library with a known vulnerability. Lacking further information, the scanner returns a positive (vulnerable) result. But if the scanner consumes SADP information that conveys that the downstream use of the library is not vulnerable (or not exploitable), then the scanner returns a negative (not vulnerable) result. Signal to noise ratio improved! So, is this assumption valid? What do vulnerability scanners think? What about other defenders who scan, either rolling their own or as users of scanning products?

Q5. How do we measure the results (success, failure)?

Q6. Should the CVE Program shift the SADP pilot into full production? How does the Program make this decision?

Q7. How long will the Pilot run?

A7. Estimates: Test period in February and March 2026, production for three months, then a formal review leading to a decision whether and how to continue.

Q8. How do I know if a CVE Record has SADP information?

A8. First, check the Record for an ADP container of type `supplier` (and/or `shortName` ends with `-SADP`). Second, we plan to provide a running list of CVE IDs that have SADP containers.
