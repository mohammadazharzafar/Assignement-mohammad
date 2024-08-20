Step  1:  Down  and unzip both compressed files
Step  2:  Go to  assignment folder and run below  terraform  cmds to  create the AWS resources
>> terraform  init
>> terraform  plan
>> terraform  apply

step  3:  verify the EKS  and connect to the cluster
>>  aws  eks  update-kubeconfig --name e-commerce-platform  -- region us-west-2

Step  4: Fix EKS cluster to create nodes.
>>  kubectl patch deployment coredns -n kube-system -p '{"spec": {"template": {"metadata": {"annotations": {"eks.amazonaws.io/psp": "eks.privileged"}}}}}'

Step  5:  Go to the k8s folder and run below cmds to run a sample application and test.
>> kubectl apply -f simple-deployment.yaml
>> kubectl get pods -n staging
>> kubectl apply -f service.yaml
>> kubectl get svc -n staging
>> kubectl run -i --tty  -n staging  load-generator --pod-running-timeout=1m0s --rm --image=busybox:1.28 --restart=Never -- sh -c  "while  sleep  1; do wget -q -O- http://php-apache; done"
ctrl+c  when  you see the "OK!" output.
>> kubectl logs deploy/php-apache -n staging --tail=10

Step 6:  Check the DynamoDB Tables
Step 7: Test the API Gateway.

Step  8:  terraform  destroy