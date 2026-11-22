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

![phpLDAPadmin showing seven OpenLDAP test accounts](https://raw.githubusercontent.com/Obeckley/IAM-Implementation-Portfolio/main/joiner-leaver/Screenshot%202026-11-18%20105305.png)

phpLDAPadmin shows seven test accounts under `ou=people` in OpenLDAP.

![OpenLDAP Accounts view in midPoint](https://raw.githubusercontent.com/Obeckley/IAM-Implementation-Portfolio/main/joiner-leaver/Screenshot%202026-11-18%20105740.png) 

The midPoint OpenLDAP Accounts view shows all seven test accounts linked to their matching midPoint users.

### Joiner and Leaver Verification

The OpenLDAP directory started with seven test accounts. The count rose
to eight after John Wick (1008) was provisioned and returned to seven
after Oliver’s account (1006) was removed.

![midPoint joiner audit event for user 1008](./joiner-audit.png)

The midPoint audit log shows a successful Add object event for John Wick
during SimplifyHR reconciliation. This is the evidence for the joiner step
because the eight-account directory view was not captured.

![midPoint leaver audit event for user 1006](./leaver-audit.png)

The midPoint audit log shows a Delete object event for Oliver’s OpenLDAP
account (`uid=1006`) during reconciliation.

![phpLDAPadmin search results after the leaver step](./post-leaver-ldap.png)

phpLDAPadmin shows seven entries under `ou=people` after the leaver step.

## Configuration Artifacts

- [OpenLDAP outbound mappings](./openldap-outbound-mappings.xml) — defines which midPoint fields populate the LDAP account.
- [LDAP DN Groovy script](./ldap-dn.groovy) — builds the account’s full directory address from the user ID and the appropriate organizational unit.
