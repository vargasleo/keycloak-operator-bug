# Composite Client Roles not being updated by Keycloak Operator (Keycloak 19.0.3-legacy)

## **Description**

We are experiencing an issue with the Keycloak Operator where the composite roles associated with a client are not being updated.

This issue consistently occurs when we create a new client. All fields in the client configuration seem to be handled correctly by the operator, except for the associated composite roles.

Manually updating these composite roles through the Keycloak UI or via direct API calls to Keycloak works correctly, and the roles appear as expected. However, the operator is not able to perform this update.

## **Environment**

**Keycloak Operator:** quay.io/keycloak/keycloak-operator:19.0.3-legacy

**Keycloak version:** quay.io/keycloak/keycloak:19.0.3-legacy

**Kubernetes Client Version:** v1.26.2

**Kubernetes Server Version:** v1.24.11-gke.1000

## **Steps to Reproduce**

1. Deploy Keycloak Operator and create a Keycloak instance
    
    Create a namespace and apply the Keycloak CRDs
    
    ```bash
    kubectl create namespace keycloak
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/19.0.3/deploy/crds/keycloak.org_keycloaks_crd.yaml -n keycloak
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/19.0.3/deploy/crds/keycloak.org_keycloakrealms_crd.yaml -n keycloak
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/19.0.3/deploy/crds/keycloak.org_keycloakclients_crd.yaml -n keycloak
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/19.0.3/deploy/crds/keycloak.org_keycloakusers_crd.yaml -n keycloak
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-operator/19.0.3/deploy/crds/keycloak.org_keycloakbackups_crd.yaml -n keycloak
    kubectl apply -f https://github.com/keycloak/keycloak-operator/raw/19.0.3/deploy/operator.yaml -n keycloak
    kubectl apply -f https://github.com/keycloak/keycloak-operator/raw/19.0.3/deploy/role_binding.yaml -n keycloak
    kubectl apply -f https://github.com/keycloak/keycloak-operator/raw/19.0.3/deploy/role.yaml -n keycloak
    kubectl apply -f https://github.com/keycloak/keycloak-operator/raw/19.0.3/deploy/service_account.yaml -n keycloak
    ```
    
    Apply the Keycloak Operator and Keycloak manifest
    
    ```bash
    kubectl apply -f keycloak-cr.yaml -n keycloak
    kubectl apply -f keycloak-operator.yaml -n keycloak
    ```
    
2. Apply the Keycloak Realm and Client manifest
    
    ```bash
    kubectl apply -f keycloak-realm.yaml -n keycloak
    kubectl apply -f keycloak-client.yaml -n keycloak
    ```
    
3. Update the client adding example_role_2 to EXAMPLE_USER composite role and apply
    
    ```bash
    kubectl apply -f keycloak-realm.yaml -n keycloak
    ```
    

## **Expected Result**

The new client is created and all roles, including composite roles, are correctly configured as per the manifest.

## **Actual Result**

The new client is created and all roles except for composite roles are correctly configured. Composite roles must be manually added through the Keycloak UI or direct API calls.

## **Additional Information**

The logs are not significant logs related to this problem. Nothing shows up related to error in reconciliation. Please let us know if there are any additional details we can provide to assist in troubleshooting this issue.

Thank you for your help.
