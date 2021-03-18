# kuber1
git clone https://github.com/GoogleCloudPlatform/kubernetes-engine-samples.git
export PROJECT_ID=amazing-zephyr-304415
echo $PROJECT_ID
cd kubernetes-engine-samples/
cd hello-app-cdn
docker build -t gcr.io/${PROJECT_ID}/hello-app-cdn:v1 .
docker images
gcloud auth configure-docker
docker push gcr.io/amazing-zephyr-304415/hello-app-cdn:v1
gcloud config set project $PROJECT_ID
gcloud config set compute/zone europe-north1-a
gcloud container clusters create homework-cluster
NAME              LOCATION         MASTER_VERSION    MASTER_IP       MACHINE_TYPE  NODE_VERSION      NUM_NODES  STATUS
homework-cluster  europe-north1-a  1.18.12-gke.1210  35.228.206.215  e2-medium     1.18.12-gke.1210  3          RUNNING

root@vm7:/home/elena.razgulayeva/kubernetes-engine-samples/hello-app-cdn# gcloud compute instances list
NAME                                             ZONE             MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
gke-homework-cluster-default-pool-8c9abc32-0x5h  europe-north1-a  e2-medium                  10.166.0.6   35.228.4.54    RUNNING
gke-homework-cluster-default-pool-8c9abc32-d4fh  europe-north1-a  e2-medium                  10.166.0.8   35.228.101.76  RUNNING
gke-homework-cluster-default-pool-8c9abc32-kn89  europe-north1-a  e2-medium                  10.166.0.7   35.228.231.17  RUNNING
vm7                                              us-central1-a    e2-medium                  10.128.0.23  34.121.8.177   RUNNING

kubectl create deployment hello-app-cdn --image=gcr.io/amazing-zephyr-304415/hello-app-cdn:v1
kubectl scale deployment hello-app-cdn --replicas=3
kubectl autoscale deployment hello-app-cdn --cpu-percent=80 --min=1 --max=5

kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
hello-app-cdn-649b87bf6b-d7ggd   1/1     Running   0          64s
hello-app-cdn-649b87bf6b-hkfb8   1/1     Running   0          28s
hello-app-cdn-649b87bf6b-rtmn8   1/1     Running   0          28s

kubectl expose deployment hello-app-cdn --name=hello-app-service --type=LoadBalancer --port 80 --target-port 8080
kubectl get service
