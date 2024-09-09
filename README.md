# megabank-gitops
 
## Instructions

1. Create two new projects - `megabank` and `megabank-be`

    ```text
    oc new-project megabank
    oc new-project megabank-be
    ```

2. Create a secret in both projects named `db2pass` that has a key-value pair containing your Db2 password.

    ```text
    oc -n megabank create secret generic --from-literal=DBPASS=<your_db2_password>
    oc -n megabank-be create secret generic --from-literal=DBPASS=<your_db2_password>
    ```
    
    *Note*: If your Db2 instance is deployed via a `db2ucluster` on the same cluster as Megabank, you can find the password as follows: 

    ```text
    oc -n db2 get secret c-db2ucluster-instancepassword -ojsonpath={.data.password} | base64 -d
    ```

3. Create routes for your `mbutil`, `megaweb`, and `mbsvc-login` services. Modify the examples [here](/routes/) by updating the domain name in the host. 

    ```text
    oc apply -f routes/
    ```

4. Deploy the Megabank frontend and backend YAML files.

    You can do this with GitOps tooling such as [ArgoCD](https://argo-cd.readthedocs.io/en/stable/), [FluxCD](https://fluxcd.io/), or [Red Hat Advanced Cluster Management](https://www.redhat.com/en/technologies/management/advanced-cluster-management).

    Alternatively, deploy them manually with: 

    ```text
    oc apply -f frontend/ -f backend/
    ```