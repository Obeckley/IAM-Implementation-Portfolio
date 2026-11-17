# HR-Driven Identity Onboarding

I connected Simplify HR to midPoint using a CSV connector and imported seven test identities.

I configured inbound mappings to tell midPoint which fields to populate from the HR records. I also set up a correlation rule to help match incoming records to existing identities and prevent duplicates.

This step covers the import from Simplify HR into midPoint. Creating accounts in OpenLDAP is the next step.

## Lab Evidence

![midPoint Users page](./Screenshot%202026-11-17%20081025.png)

The midPoint Users page shows seven test identities imported from Simplify HR.

![midPoint audit log](./Screenshot%202026-11-17%20081221.png)

The midPoint audit log is filtered to the day of the HR import.
