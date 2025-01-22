# Connect to Postgres database from Python application deployed on Kubernetes using configmaps & secrets

## Description
A brief description of what the project does and who it's for.

## Installation
Instructions on how to install and set up the project.

```bash
# Clone the repository
git clone https://github.com/yourusername/python-db-k8s.git

# Navigate to the project directory
cd python-db-k8s

# Create docker image 
docker build -t python-postgres-k8s-test:1.0 .

# Push docker image to docker registry
docker tag python-postgres-k8s-test:1.0 <dockerhub-user>/python-postgres-k8s-test:1.0
docker push chailearngke/python-postgres-k8s-test:1.0
```

## Deployment to EKS
Steps to deploy the application to Amazon EKS.

1. **Create an EKS Cluster:**
   ```bash
   eksctl create cluster --name my-cluster --region us-west-2
   ```

2. **Configure kubectl:**
   ```bash
   aws eks --region us-west-2 update-kubeconfig --name my-cluster
   ```
3. **ConfigMap**
   Create a ConfigMap for the database name, port and user:
   ```sh
   kubectl apply -f db-configmap.yaml
   ```

4. **Secrets**
   Create a Secrets for the database password:
   ```sh
   kubectl apply -f db-secrets.yaml
   ```
5. **Deployment & Service**
   Create deployment & services
   ```sh
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   ```

## Establish PostgreSQL DB Connection in RDS
Steps to establish a PostgreSQL database connection in Amazon RDS.

1. **Create a PostgreSQL DB Instance:**
   - Go to the RDS console.
   - Click on "Create database".
   - Select "PostgreSQL" and configure the instance settings.

2. **Configure Security Group:**
   - Ensure the security group allows inbound traffic on the PostgreSQL port (default 5432).

3. **Connect to the DB:**
   ```python
   import psycopg2

   connection = psycopg2.connect(
       host="your-rds-endpoint",
       database="your-database-name",
       user="your-username",
       password="your-password"
   )
   ```

## Accessibility
Ensure the application is accessible.

**Access the Application:**
   - Retrieve the external IP address of the service.
   - Open a web browser and navigate to the external IP.
     http://<external-ip>/db-test
