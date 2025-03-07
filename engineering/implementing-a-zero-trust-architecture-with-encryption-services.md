---
tags:
  - zero-trust
  - vault
  - security
  - encryption
title: Implementing a Zero Trust Architecture with Encryption Services
date: 2025-03-10
description: This article explores the implementation of a zero trust architecture, a security model that assumes no trust and verifies every access request. It delves into the critical role of encryption services in safeguarding data within this framework. By focusing on these core principles, the article provides insights applicable to a wide range of security contexts.
authors:
  - quang
---

# Implementing a Zero Trust System with HashiCorp Vault and GCP KMS on Kubernetes

In modern distributed systems, traditional security models relying on a trusted perimeter are insufficient. The **zero trust security model**, based on the principle of "never trust, always verify," ensures that every access request is authenticated and authorized, regardless of its origin. This article provides a comprehensive guide to implementing a zero trust system using **HashiCorp Vault** and **Google Cloud Platform's Key Management Service (GCP KMS)** in a **Kubernetes (K8s)** environment. This setup offers a secure and scalable framework for protecting sensitive data and resources.


## Understanding Zero Trust

Zero trust assumes that no user, device, or service—whether inside or outside the network—can be trusted by default. Every request must be verified based on identity, context, and behavior. This is especially critical in Kubernetes, where workloads are dynamic and ephemeral. A zero trust system enhances security by:

- Eliminating implicit trust.
- Enforcing strict access controls.
- Encrypting data at every stage.


## Leveraging HashiCorp Vault for Zero Trust

**HashiCorp Vault** is a cornerstone of zero trust architectures, providing **secrets management**, **encryption services**, and **access control**. Vault centralizes sensitive data—like passwords, API keys, and certificates—and ensures only authorized entities can access them. Key features supporting zero trust include:

- **Policy-based access control**: Fine-grained rules determine who or what can interact with Vault.
- **Encryption as a service**: Secures data without exposing encryption keys to applications.
- **Audit logging**: Tracks all actions for accountability and compliance.

## Preparing GCP Credentials for Vault

Before deploying Vault on Kubernetes, you need to create a service account in Google Cloud Platform (GCP) and generate a credentials file (`gcp-credentials.json`). This file enables Vault to authenticate with GCP KMS for the auto-unseal process. Follow these steps to prepare the credentials:

#### Create a Service Account in GCP

1. **Navigate to the GCP Console**:
   - Open the [Google Cloud Console](https://console.cloud.google.com/).
   - Select your project (e.g., `my-project`).

2. **Create a Service Account**:
   - In the left-hand menu, go to **IAM & Admin** > **Service Accounts**.
   - Click **Create Service Account**.
   - Enter a name (e.g., `vault-gcp-kms`) and a description (e.g., "Service account for Vault auto-unseal with GCP KMS").
   - Click **Create**.

3. **Assign Permissions**:
   - On the **Grant this service account access to project** page, assign the `Cloud KMS CryptoKey Encrypter/Decrypter` role. This role allows Vault to use the KMS key for encryption and decryption during unsealing.
   - Click **Continue**, then **Done**.

#### Generate the Credentials File

1. **Create a Key for the Service Account**:
   - In the **Service Accounts** list, locate your newly created service account (e.g., `vault-gcp-kms`).
   - Click the three dots on the right and select **Manage keys**.
   - Click **Add Key** > **Create new key**.
   - Select **JSON** as the key type and click **Create**.
   - A file (e.g., `<project-id>-<unique-id>.json`) will be downloaded to your local machine.

2. **Rename the File**:
   - Rename the downloaded file to `gcp-credentials.json` for consistency with Vault’s configuration.

#### Store the Credentials in a Kubernetes Secret

To securely provide the credentials to Vault, store the `gcp-credentials.json` file in a Kubernetes secret.

1. **Create the Secret**:
   - Run the following command to create a secret named `vault-gcp-credentials`:
     ```bash
     kubectl create secret generic vault-gcp-credentials --from-file=gcp-credentials.json
     ```
   - This command creates a secret with the key `gcp-credentials.json` containing the file’s contents.

2. **Verify the Secret**:
   - Confirm the secret was created successfully:
     ```bash
     kubectl get secret vault-gcp-credentials
     ```

#### Notes
- Keep the `gcp-credentials.json` file secure and avoid exposing it in public repositories or shared environments.
- Ensure the `vault-gcp-credentials` secret is created in the same Kubernetes namespace where Vault will be deployed.

## Deploying Vault on Kubernetes

To deploy Vault securely in a Kubernetes cluster, we use the official **Vault Helm chart**. The configuration below sets up a standalone Vault server with production-ready features, including TLS, GCP KMS auto-unseal, and persistent storage.

### Configuration Details

Here’s the `values.yaml` configuration for the deployment:

```yaml
global:
  enabled: true
  tlsDisable: false  # Enable TLS for production

injector:
  enabled: false

server:
  # Enable standalone mode
  standalone:
    enabled: true
    config: |
      ui = true

      listener "tcp" {
        address = "[::]:8200"
        tls_disable = true
      }

      storage "raft" {
        path = "/vault/data"
      }

      seal "gcpckms" {
        project    = "project-id"
        region     = "global"
        key_ring   = "vault-key-ring"
        crypto_key = "vault-unseal-key"
      }

  readinessProbe:
    enabled: false

  # Disable HA mode
  ha:
    enabled: false

  # Configure Persistent Volume
  dataStorage:
    enabled: true
    size: 10Gi
    storageClass: "standard"  # Replace with a valid StorageClass or leave empty for default
    accessMode: "ReadWriteOnce"
    mountPath: "/vault/data"  # Must match the Raft storage path

  # Environment variables for GCP credentials
  extraEnvironmentVars:
    GOOGLE_APPLICATION_CREDENTIALS: "/vault/userconfig/gcp-credentials.json"

  # Mount secrets as volumes
  volumes:
    - name: gcp-credentials
      secret:
        secretName: "vault-gcp-credentials"

  volumeMounts:
    - name: gcp-credentials
      mountPath: "/vault/userconfig/gcp-credentials.json"
      subPath: "gcp-credentials.json"
      readOnly: true

  # Filesystem permissions
  securityContext:
    runAsUser: 100
    runAsGroup: 1000
    fsGroup: 1000

# Enable Vault UI
ui:
  enabled: true
  serviceType: "ClusterIP"
```

### Deployment Steps

1. **Prepare the Configuration**:
   - Save the configuration in a file named `values.yaml`.

2. **Add the HashiCorp Helm Repository**:
   - Run these commands:
     ```bash
     helm repo add hashicorp https://helm.releases.hashicorp.com
     helm repo update
     ```

3. **Install Vault**:
   - Deploy Vault using the Helm chart:
     ```bash
     helm install vault hashicorp/vault -f values.yaml
     ```

4. **Verify the Deployment**:
   - Check the Vault pod status:
     ```bash
     kubectl get pods
     ```
   - Confirm auto-unseal by reviewing logs:
     ```bash
     kubectl logs <vault-pod-name>
     ```

This setup delivers a secure, standalone Vault deployment on Kubernetes with GCP KMS auto-unseal and persistent storage, aligning with zero trust principles.


## Integrating GCP KMS for Auto-Unseal

Vault requires an "unseal" process to access encrypted data after a restart. Manual unsealing is impractical in Kubernetes due to frequent pod restarts. **GCP KMS** enables **auto-unseal**, allowing Vault to unlock itself using securely managed keys in Google Cloud.

### Configuration Steps

1. **Create KMS Resources**:
   - In GCP, create a key ring (e.g., `vault-key-ring`) and a symmetric key (e.g., `vault-unseal-key`).

2. **Assign Permissions**:
   - Grant the Vault service account (linked to a Kubernetes service account) the `roles/cloudkms.cryptoKeyEncrypterDecrypter` IAM role.

3. **Update Vault Configuration**:
   - The `seal "gcpckms"` block in `values.yaml` configures auto-unseal with GCP KMS.

This integration reduces operational overhead while upholding high security standards.


## Encrypting Data with Vault Transit

The **Vault Transit secrets engine** provides **encryption as a service**, enabling applications to encrypt and decrypt data without managing keys directly. This keeps sensitive information secure.

### Setup Process

1. **Enable Transit Engine**:
     - Activate the Transit engine:
         ```bash
         vault secrets enable transit
         ```

2. **Generate an Encryption Key**:
  - **Create a Vault-managed key**:
      ```bash
      vault write transit/keys/my-key type="aes256-gcm96" exportable=false
      ```

  - **Encrypt and Decrypt Data**:
      - Encrypt:
        ```bash
        vault write transit/encrypt/my-key plaintext=$(echo -n "plain_text" | base64)
        ```
      - Decrypt it:
        ```bash
        vault write transit/decrypt/my-key ciphertext="vault:v1:..."
        ```
    
3. **Store Encrypted Data**:
- Applications can store encrypted data in databases, env or other storage systems. Decryption is only possible via Vault’s API.

This simplifies key management and ensures data security across applications.


## Enhancing Security for Zero Trust

A robust zero trust system requires additional protections:

- **Authentication**:
  - Enable Kubernetes authentication in Vault:
    ```bash
    vault auth enable kubernetes
    ```
  - Next, configure Vault to communicate with your Kubernetes cluster:
    ```bash
    vault write   auth/kubernetes/config \
      kubernetes_host="https://kubernetes.default.svc:443" \
      kubernetes_ca_cert=@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt \
      token_reviewer_jwt=@/var/run/secrets/kubernetes.io/serviceaccount/token
    ```
  - Define a Vault Policy to control what the application can access. For example:
    ```bash
    vault policy write my-app-policy - <<EOF
    path "kv/data/my-app/*" {
      capabilities = ["read"]
    }
    path "transit/encrypt/my-key*" {
      capabilities = ["create", "update"]
    }
    path "transit/decrypt/my-key*" {
      capabilities = ["read", "update"]
    }
    path "transit/keys/my-key*" {
      capabilities = ["create", "update", "read", "delete"]
    }
    EOF
    ```
  - Bind the Policy to a Kubernetes Service Account.
    ```bash
    vault write auth/kubernetes/role/my-app-role \
        bound_service_account_names=SERVICE_ACCOUNT_NAME1,SERVICE_ACCOUNT_NAME2 \
        bound_service_account_namespaces=NAMESPACE1,NAMESPACE2 \
        policies=my-app-policy \
        ttl=24h
    ```
    - SERVICE_ACCOUNT_NAME: The service account name used by your pod.
    - NAMESPACE: The Kubernetes namespace of the service account.
    - ttl=24h: The Vault token’s time-to-live (adjust as needed).

- **Monitoring**:
  - Enable audit logging:
    ```bash
    vault audit enable file file_path=/vault/logs/audit.log
    ```
  - Monitor logs with tools like Prometheus for real-time insights.

These measures ensure every interaction is authenticated, encrypted, and auditable.

## Accessing Vault from Kubernetes Applications

### Overview of the Approach

The Kubernetes auth method in Vault allows a pod to authenticate using its service account token, which is automatically mounted at `/var/run/secrets/kubernetes.io/serviceaccount/token`. The application sends this token to Vault’s `/auth/kubernetes/login` endpoint along with a role name. Vault verifies the token with the Kubernetes API and, if successful, issues a Vault token that the application can use for subsequent interactions with Vault.

### Implementation Steps

Here’s how to implement this in your Golang application:

#### 1. Read the Kubernetes Service Account Token
The service account token is mounted in every Kubernetes pod at `/var/run/secrets/kubernetes.io/serviceaccount/token`. Read this file to obtain the JWT token.

#### 2. Authenticate with Vault
Send a POST request to Vault’s `/auth/kubernetes/login` endpoint with the token and the role name. Vault will respond with a Vault token.

#### 3. Use the Vault Token
Initialize a Vault client with the obtained token and use it to interact with Vault (e.g., read secrets or encrypt data).

### Complete Golang Code

#### Retrieve KV Secrets Using Go

```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"log"
	"net/http"

	"github.com/hashicorp/vault/api"
)

// VaultLoginResponse represents the response from Vault's login endpoint
type VaultLoginResponse struct {
	Auth struct {
		ClientToken string `json:"client_token"`
	} `json:"auth"`
}

func main() {
	// Step 1: Read the Kubernetes service account token
	k8sTokenPath := "/var/run/secrets/kubernetes.io/serviceaccount/token"
	k8sToken, err := ioutil.ReadFile(k8sTokenPath)
	if err != nil {
		log.Fatalf("Failed to read Kubernetes service account token: %v", err)
	}

	// Step 2: Prepare the login request to Vault
	vaultAddr := "http://vault:8200" // Replace with your Vault server address
	role := "my-app-role"            // Replace with your Vault role name

	loginData := map[string]interface{}{
		"jwt":  string(k8sToken),
		"role": role,
	}
	loginDataJSON, err := json.Marshal(loginData)
	if err != nil {
		log.Fatalf("Failed to marshal login data: %v", err)
	}

	// Step 3: Send the login request to Vault
	resp, err := http.Post(vaultAddr+"/v1/auth/kubernetes/login", "application/json", bytes.NewBuffer(loginDataJSON))
	if err != nil {
		log.Fatalf("Failed to send login request: %v", err)
	}
	defer resp.Body.Close()

	// Step 4: Check the response and extract the Vault token
	if resp.StatusCode != http.StatusOK {
		log.Fatalf("Login failed with status: %s", resp.Status)
	}

	var loginResp VaultLoginResponse
	if err := json.NewDecoder(resp.Body).Decode(&loginResp); err != nil {
		log.Fatalf("Failed to decode login response: %v", err)
	}

	vaultToken := loginResp.Auth.ClientToken
	if vaultToken == "" {
		log.Fatal("No Vault token received")
	}

	// Step 5: Initialize the Vault client with the token
	client, err := api.NewClient(&api.Config{
		Address: vaultAddr,
	})
	if err != nil {
		log.Fatalf("Failed to create Vault client: %v", err)
	}
	client.SetToken(vaultToken)

	// Step 6: Example - Read a secret from Vault
	secret, err := client.Logical().Read("kv/data/my-app/config")
	if err != nil {
		log.Fatalf("Failed to read secret: %v", err)
	}

	if secret != nil && secret.Data != nil {
		data, ok := secret.Data["data"].(map[string]interface{})
		if ok {
			fmt.Printf("Secret data: %v\n", data)
		} else {
			fmt.Println("Secret data not found or invalid format")
		}
	} else {
		fmt.Println("No secret found")
	}
}
```

#### Use the Transit Secrets Engine in Go

```go
package main

import (
	"fmt"
	"log"
	"os"
	"github.com/hashicorp/vault/api"
)

func main() {
	// Initialize Vault client
	....

  vaultToken := loginResp.Auth.ClientToken
	if vaultToken == "" {
		log.Fatal("No Vault token received")
	}

  // Initialize the Vault client with the token
	client, err := api.NewClient(&api.Config{
		Address: vaultAddr,
	})
	if err != nil {
		log.Fatalf("Failed to create Vault client: %v", err)
	}
	client.SetToken(vaultToken)

	// Encrypt data
	ciphertext := os.Getenv("ENCRYPTED_DATA")

	// Decrypt data
	decryptData := map[string]interface{}{
		"ciphertext": ciphertext,
	}
	decryptResponse, err := client.Logical().Write("transit/decrypt/my-key", decryptData)
	if err != nil {
		log.Fatalf("Failed to decrypt data: %v", err)
	}
	decryptedPlaintext := decryptResponse.Data["plaintext"].(string)
	fmt.Printf("Decrypted plaintext: %s\n", decryptedPlaintext)
}
```

Here is an example of a Kubernetes Deployment manifest for the Golang application:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  namespace: NAMESPACE1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      serviceAccountName: SERVICE_ACCOUNT_NAME1
      containers:
      - name: my-app
        image: myregistry/myapp:latest
```

Create a Kubernetes Service Account:

```bash
kubectl create serviceaccount SERVICE_ACCOUNT_NAME1 -n NAMESPACE1
```

Deploy the application:

```bash
kubectl apply -f deployment.yaml
```

---

### Security Considerations

- **TLS**: If Vault uses HTTPS, update `vaultAddr` to `https://...` and configure the client to handle TLS certificates (e.g., via `api.Config.HttpClient`).
- **Token Lifetime**: The Vault token is short-lived. For long-running applications, implement renewal logic using `client.Auth().Token().RenewSelf`.
- **Error Handling**: Add retries or more robust error handling for production use.


## Conclusion

Combining HashiCorp Vault with GCP KMS in a Kubernetes environment enables organizations to implement a zero trust system that secures data and resources in dynamic, distributed architectures. Vault’s deployment on Kubernetes, auto-unseal with GCP KMS, and Transit engine for encryption form a comprehensive framework for enforcing zero trust principles. This approach is adaptable across industries, delivering robust security and operational efficiency where trust must be earned, not assumed.