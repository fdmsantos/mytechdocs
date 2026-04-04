# GCP Onboarding

## Troubleshooting

### Service Account not in permitted organization

When applying the GCP onboarding Terraform, the following error may occur:

```
Error: Error applying IAM policy for organization: googleapi: Error 400: 
One or more users named in the policy do not belong to a permitted customer.
"User cortex-engine@outpost-xdr-eu-xxx.iam.gserviceaccount.com is not in permitted organization."
type: constraints/iam.allowedPolicyMemberDomains
```

**Cause:** The org policy `iam.allowedPolicyMemberDomains` restricts which domains can receive IAM bindings in the organization. The Cortex service account is external and is blocked by this constraint.

**Solution:** Add the Palo Alto customer ID to the allowed domains list in the `iam.allowedPolicyMemberDomains` org policy.

### VPC Service Controls perimeter blocking Cortex Cloud

A service perimeter provides an additional layer of security for GCP projects, acting as a boundary that prevents unauthorized communication to Google Cloud services beyond its confines. To enable Cortex Cloud to scan assets within the perimeter, you must authorize Cortex Cloud's identities to access it.

If the perimeter is not authorized, you will receive the following error:

```
Request is prohibited by organization's policy. vpcServiceControlsUniqueIdentifier: {{<GCP-perimeter-ID>}}
```

**Solution:** Authorize Cortex Cloud's identities to access the perimeter. See [Monitor GCP resources inside service perimeters](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Monitor-GCP-resources-inside-service-perimeters?tocId=Cgb8cKml6wLPPkEeL~qzzA).