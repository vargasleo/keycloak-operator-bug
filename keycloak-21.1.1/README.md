# Composite Client Roles not being updated by Keycloak Operator (Keycloak 21.1.1)

## **Description**

We are experiencing an issue with the Keycloak Operator where the composite roles associated with a client are not being updated.

This issue consistently occurs when we create a new client. All fields in the client configuration seem to be handled correctly by the operator, except for the associated composite roles.

Manually updating these composite roles through the Keycloak UI or via direct API calls to Keycloak works correctly, and the roles appear as expected. However, the operator is not able to perform this update.

## **Environment**

**Keycloak Operator:** quay.io/keycloak/keycloak:21.1.1

**Keycloak version:** quay.io/keycloak/keycloak-operator:21.1.1

**Kubernetes Client Version:** v1.26.2

**Kubernetes Server Version:** v1.24.11-gke.1000

## **Steps to Reproduce**

1. Deploy Keycloak Operator and create a Keycloak instance
    
    Create a namespace and apply the Keycloak CRDs
    
    ```bash
    kubectl create namespace keycloak
    #https://www.keycloak.org/operator/installation
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/21.1.1/kubernetes/keycloaks.k8s.keycloak.org-v1.yml
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/21.1.1/kubernetes/keycloakrealmimports.k8s.keycloak.org-v1.yml
    kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/21.1.1/kubernetes/kubernetes.yml
    ```
    
    Apply the DB, Operator and Keycloak manifest
    
    ```bash
    kubectl apply -f keycloak-operator.yaml -n keycloak
    kubectl apply -f example-postgres.yaml -n keycloak
    kubectl apply -f example-db-secret.yaml -n keycloak
    kubectl apply -f keycloak-cr.yaml -n keycloak
    ```
    
2. Apply the Keycloak Realm and Client manifest
    
    ```bash
    kubectl apply -f keycloak-realm.yaml -n keycloak
    ```
    
3. Find and update the client adding count1-count2 to count2-count3 composite role and apply
    
    ```yaml
    - id: 420e6995-c90b-446a-b8a2-ec2acea2b1fa
              name: count2-count3
              description: "Composite role for count2"
              composite: true
              composites:
                client:
                  count1:
                  - count1-count1
    		    - count1-count2 # add this line
    ```
    

## **Expected Result**

The new associate role to count2-count3 composite role is correctly configured as per the manifest.

## **Actual Result**

The new associate role is not updated and no errors logs are shown. Composite roles must be manually added through the Keycloak UI or direct API calls.

## **Additional Information**

The logs are not significant logs related to this problem. Nothing shows up related to error in reconciliation. Please let us know if there are any additional details we can provide to assist in troubleshooting this issue.

Thank you for your help.
