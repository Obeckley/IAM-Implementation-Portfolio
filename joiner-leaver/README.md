# HR-Driven Identity Onboarding

I connected Simplify HR to midPoint using a CSV connector and imported seven test identities.

I configured inbound mappings to tell midPoint which fields to populate from the HR records. I also set up a correlation rule to help match incoming records to existing identities and prevent duplicates.

## From HR to the directory

After importing seven test identities from SimplifyHR, I connected OpenLDAP as the target directory. I configured the Employee role to provision an OpenLDAP account and used SimplifyHR Reconciliation to test automatic role assignment.

Outbound mappings tell midPoint which fields to send to OpenLDAP, like the employee number, name, email, and department. The DN mapping uses Groovy to build the account’s full address in the directory.

I checked user `1001` in midPoint and confirmed that SimplifyHR Reconciliation assigned the Employee role. I then verified that the matching `uid=1001` account existed in OpenLDAP with the expected fields populated.

## Lab Evidence

![midPoint Users page](./Screenshot%202026-11-17%20081025.png)

The midPoint Users page shows seven test identities imported from Simplify HR.

![midPoint audit log](./Screenshot%202026-11-17%20081221.png)

The midPoint audit log is filtered to the day of the HR import.
