# ConfigMap and Secrets in Kubernetes

In Kubernetes, ConfigMaps and Secrets are used to manage application configurations and sensitive data, respectively. Let's explore why we use ConfigMaps and Secrets, their differences, and how to use them in a demo project.

## Why Use ConfigMap?

**Problem**: Storing application configurations that may need to be changed or updated without modifying the application code.

**Solution**: ConfigMap provides a way to store key-value pairs or configuration files that can be consumed by applications running in Kubernetes pods.

## Why Use Secrets?

**Problem**: Storing sensitive data such as passwords, API keys, and tokens securely.

**Solution**: Secrets provide a way to store and manage sensitive information in a secure manner, ensuring that it is encrypted at rest and only accessible to authorized users and applications.

## Steps Followed by Secrets:

1. **Create Secret**: Define the sensitive data and store it securely in a Kubernetes Secret object.
2. **Mount Secret**: Mount the Secret into the desired pods as volumes or environment variables.
3. **Access Secret**: Access the sensitive data within the application running in the pod.

## Difference between ConfigMap and Secrets:

| Aspect          | ConfigMap                         | Secrets                            |
|-----------------|---------------------------------- |------------------------------------|
| **Data Type**   | Non-sensitive data                | Sensitive data                     |
| **Storage**     | Plain text or configuration files | Encrypted and secure               |
| **Use Cases**   | Application configurations        | Passwords, API keys, tokens, etc.  |
| **Access Level**| Publicly accessible               | Restricted access                  |

## Demo Project: ConfigMap

### Steps:

1. Create ConfigMap:
   ```bash
   kubectl create configmap my-config --from-literal=DATABASE_URL=mysql://localhost:3306/mydb
   ```

2. Deploy Application:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-app
   spec:
     containers:
     - name: my-app-container
       image: my-app-image
       envFrom:
       - configMapRef:
           name: my-config
   ```

## Demo Project: Secrets

### Steps:

1. Create Secret:
   ```bash
   kubectl create secret generic my-secret --from-literal=DB_USERNAME=myuser --from-literal=DB_PASSWORD=mypassword
   ```

2. Deploy Application:
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-app
   spec:
     containers:
     - name: my-app-container
       image: my-app-image
       envFrom:
       - secretRef:
           name: my-secret
   ```

## Conclusion:

ConfigMaps and Secrets are essential Kubernetes resources for managing application configurations and sensitive data. By understanding their differences and following the steps outlined above, you can effectively manage configurations and secrets in your Kubernetes deployments, ensuring security and flexibility for your applications.