# eks-cronjob-to-patch-hpa
EKS cronjob to patch HPA

**1. Install Metric server**

    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

    # kubectl get deployment metrics-server -n kube-system
    NAME             READY   UP-TO-DATE   AVAILABLE   AGE
    metrics-server   0/1     1            0           26s


**2. Install Deployment and Service for Nginx**

    # k apply -f nginx_deploy.yaml 
    deployment.apps/nginx-deployment created

    # k delete apply -f nginx-service.yaml 
    error: when paths, URLs, or stdin is provided as input, you may not specify resource arguments as well


**3. Install HPA for Nginx**

    # k apply -f hpa.yaml 
    horizontalpodautoscaler.autoscaling/nginx-deployment configured

**4. Install Cronjob for scale up (set time according to your current timezone.)**

    # k apply -f cronjob.yaml 
    cronjob.batch/nginx-deployment-scale-up configured
    
    # k get cronjob
    NAME                                SCHEDULE     SUSPEND   ACTIVE   LAST SCHEDULE   AGE
    nginx-deployment-scale-up           51 3 * * *   False     1        34s             45s


    # k get pods
    NAME                                       READY   STATUS      RESTARTS   AGE
    nginx-deployment-9456bbbf9-jprnx           1/1     Running     0          4s
    nginx-deployment-9456bbbf9-qzb88           1/1     Running     0          40m
    nginx-deployment-scale-up-27479751-zrv84   0/1     Completed   0          6s
