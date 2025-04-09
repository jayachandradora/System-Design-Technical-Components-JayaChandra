# Dynamic Auto Scalling based on Situation.

### Use Case & Final Workflow:
1. From 10 AM to 6 PM, the CronJobs will ensure that the minimum replica count is 30 and the maximum replica count is 50.
2. From 6 PM to 10 AM, the CronJobs will ensure that the minimum replica count is 10 and the maximum replica count is 20.
3. The HPA will still scale based on CPU utilization (or any other metrics like memory) within the specified range.

The scale-up-kafka-consumer CronJob will run at 10 AM and set the replica count to 30.
The scale-down-kafka-consumer CronJob will run at 6 PM and set the replica count to 10.
The scale-back-kafka-consumer CronJob will run at 8 PM and set the replica count to 20.

### CronJob Schedule Syntax:
"0 10 * * *" means "run at 10 AM every day".
"0 18 * * *" means "run at 6 PM every day".
"0 20 * * *" means "run at 8 PM every day".
kubectl scale command used in the CronJob is simple and relies on the bitnami/kubectl image. If you have your custom containers, you can adjust accordingly.

Certainly! Here's a suggested folder structure and a complete set of YAML files that you can use to implement the solution.

### Folder Structure

```
k8s/
├── deployment/
│   └── kafka-consumer-deployment.yaml
├── hpa/
│   └── kafka-consumer-hpa.yaml
├── cronjobs/
│   ├── scale-up-kafka-consumer.yaml
│   ├── scale-down-kafka-consumer.yaml
│   └── scale-back-kafka-consumer.yaml
```

### Detailed Explanation of Each File

1. **`k8s/deployment/kafka-consumer-deployment.yaml`** - This file contains the deployment configuration for your Kafka consumer.

2. **`k8s/hpa/kafka-consumer-hpa.yaml`** - This file defines the Horizontal Pod Autoscaler (HPA) for auto-scaling based on resource utilization (CPU).

3. **`k8s/cronjobs/scale-up-kafka-consumer.yaml`** - This file defines the CronJob to scale up the Kafka consumer replicas to 30 during peak hours (10 AM).

4. **`k8s/cronjobs/scale-down-kafka-consumer.yaml`** - This file defines the CronJob to scale down the Kafka consumer replicas to 10 during off-peak hours (6 PM).

5. **`k8s/cronjobs/scale-back-kafka-consumer.yaml`** - This file defines the CronJob to scale the replicas back to 20 after off-peak hours (8 PM).

### Complete YAML Files

#### 1. **`k8s/deployment/kafka-consumer-deployment.yaml`**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kafka-consumer
  labels:
    app: kafka-consumer
spec:
  replicas: 10  # Default minimum replicas (will be dynamically adjusted by CronJobs)
  selector:
    matchLabels:
      app: kafka-consumer
  template:
    metadata:
      labels:
        app: kafka-consumer
    spec:
      containers:
        - name: kafka-consumer
          image: your-consumer-image  # Replace with your actual Kafka consumer image
          resources:
            requests:
              cpu: "100m"
              memory: "200Mi"
            limits:
              cpu: "500m"
              memory: "1Gi"
```

#### 2. **`k8s/hpa/kafka-consumer-hpa.yaml`**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: kafka-consumer-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: kafka-consumer  # Name of your Kafka consumer deployment
  minReplicas: 10  # Minimum replicas set by CronJobs
  maxReplicas: 50  # Maximum replicas set by CronJobs
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50  # Auto-scale when CPU utilization exceeds 50%
```

#### 3. **`k8s/cronjobs/scale-up-kafka-consumer.yaml`**
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: scale-up-kafka-consumer
spec:
  schedule: "0 10 * * *"  # At 10 AM every day
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: scale-up
              image: bitnami/kubectl:latest  # kubectl image to run scale command
              command:
                - /bin/bash
                - -c
                - kubectl scale deployment kafka-consumer --replicas=30
          restartPolicy: OnFailure
```

#### 4. **`k8s/cronjobs/scale-down-kafka-consumer.yaml`**
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: scale-down-kafka-consumer
spec:
  schedule: "0 18 * * *"  # At 6 PM every day
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: scale-down
              image: bitnami/kubectl:latest  # kubectl image to run scale command
              command:
                - /bin/bash
                - -c
                - kubectl scale deployment kafka-consumer --replicas=10
          restartPolicy: OnFailure
```

#### 5. **`k8s/cronjobs/scale-back-kafka-consumer.yaml`**
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: scale-back-kafka-consumer
spec:
  schedule: "0 20 * * *"  # At 8 PM every day
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: scale-back
              image: bitnami/kubectl:latest  # kubectl image to run scale command
              command:
                - /bin/bash
                - -c
                - kubectl scale deployment kafka-consumer --replicas=20
          restartPolicy: OnFailure
```

### How to Apply the Files to Your Kubernetes Cluster

1. **Create the required directories:**
   ```bash
   mkdir -p k8s/deployment k8s/hpa k8s/cronjobs
   ```

2. **Save the respective YAML files** into the directories as shown in the folder structure above.

3. **Apply the YAML files** to your Kubernetes cluster using `kubectl apply`:

   - Apply the **Deployment**:
     ```bash
     kubectl apply -f k8s/deployment/kafka-consumer-deployment.yaml
     ```

   - Apply the **Horizontal Pod Autoscaler**:
     ```bash
     kubectl apply -f k8s/hpa/kafka-consumer-hpa.yaml
     ```

   - Apply the **CronJobs**:
     ```bash
     kubectl apply -f k8s/cronjobs/scale-up-kafka-consumer.yaml
     kubectl apply -f k8s/cronjobs/scale-down-kafka-consumer.yaml
     kubectl apply -f k8s/cronjobs/scale-back-kafka-consumer.yaml
     ```

4. **Verify the CronJobs and HPA**:
   - Check if the **CronJobs** are scheduled:
     ```bash
     kubectl get cronjobs
     ```

   - Check if the **HPA** is working:
     ```bash
     kubectl get hpa
     ```

5. **Monitor the scaling**: You can monitor the scaling of your **kafka-consumer** deployment using:
   ```bash
   kubectl get deployment kafka-consumer
   ```

6. **Test Auto-Scaling**:
   - During the set time intervals, the replicas should adjust based on the CronJobs.
   - When CPU usage exceeds the threshold defined in the HPA (50%), it should scale based on the resource utilization.

### Final Notes

- **Images:** Replace the `your-consumer-image` placeholder with the actual Kafka consumer Docker image you're using.
- **kubectl Image:** The `bitnami/kubectl:latest` image is used in the CronJobs to execute the scaling commands. You can replace it with any other image that includes `kubectl`, or use your custom image.
- **Time Zones:** The `CronJobs` are based on the UTC time zone. If you're using a different time zone for your application, you may need to adjust the schedule accordingly or handle time zone conversions.

This solution allows your Kafka consumers to scale based on time of day, while also leveraging HPA for load-based scaling.
