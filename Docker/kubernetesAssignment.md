Certainly! Here are ten practical questions that cover various tasks you might encounter in the Certified Kubernetes Application Developer (CKAD) exam:

**Question 1: Create a Namespace**
Create a new Kubernetes namespace named "my-namespace."

**Question 2: Create a Pod**
Create a Pod named "nginx-pod" in the "my-namespace" namespace running an Nginx container. Ensure that it is in a "Running" state.

**Question 3: Deploy an Application**
Deploy a stateless application named "my-app" using the "my-namespace" namespace, which runs three replicas. Use the "nginx" container image.

**Question 4: Expose the Application**
Create a Service to expose the "my-app" application using a ClusterIP service type in the "my-namespace." The service should listen on port 80 and target the application's pods.

**Question 5: Scale the Application**
Scale the "my-app" deployment to have five replicas in the "my-namespace."

**Question 6: Update the Application**
Update the "my-app" deployment to use a new image version, "nginx:1.19.10." Ensure that the change is rolled out without disrupting the service.

**Question 7: Create a ConfigMap**
Create a ConfigMap named "my-config" with two key-value pairs: "DB_HOST" set to "mydb" and "DB_PORT" set to "3306."

**Question 8: Mount ConfigMap in a Pod**
Create a new Pod named "configmap-pod" in the "my-namespace" namespace that uses the "nginx" container image. Mount the "my-config" ConfigMap as a volume at "/etc/config" and expose the key-value pairs as environment variables.

**Question 9: Implement Resource Constraints**
Modify the "nginx-pod" created earlier to limit the CPU and memory resources to 0.5 CPU cores and 256MiB of memory.

**Question 10: Create an Ingress Resource**
Create an Ingress resource named "my-ingress" in the "my-namespace" namespace, directing traffic to the "my-app" service on path "/my-app" with the hostname "myapp.example.com."

These practical questions cover a range of tasks you might perform during the CKAD exam, including creating resources, managing applications, configuring services, working with configuration data, setting resource constraints, and handling network traffic. Practice these tasks to build your skills and confidence for the exam.
