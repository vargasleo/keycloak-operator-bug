# Composite Client Roles not being updated by Keycloak Operator

## **Description**

We are experiencing an issue with the Keycloak Operator where the composite roles associated with a client are not being updated.

This issue consistently occurs when we create a new client. All fields in the client configuration seem to be handled correctly by the operator, except for the associated composite roles.

Manually updating these composite roles through the Keycloak UI or via direct API calls to Keycloak works correctly, and the roles appear as expected. However, the operator is not able to perform this update.

I've tested it both with 19.0.3-legacy and 21.1.1 versions.