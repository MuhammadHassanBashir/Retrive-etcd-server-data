# Retrive-etcd-server-data

I have a GKE (Google Kubernetes Engine) cluster in Google Cloud, where I manually deployed an Etcd server as a StatefulSet. 

To access the Etcd pod, I used the following command:


    kubectl exec -it pod/<etcd-podname> -- /bin/sh

Once inside the pod, I ran the following command to view the Etcd data:

    etcdctl --user root:Etcd123$%^... get "" --prefix

By default, Etcd was set to use the "root" user. I obtained the password by assuming that the Etcd StatefulSet was deployed using a Helm chart. Therefore, I retrieved the values.yaml file for Etcd from the running StatefulSet in the cluster using the following command:


    helm get values statefulsets/etcd-deployment -n default > etcd-values.yaml

In the values.yaml, I found the Etcd password, which I then used in the etcdctl command.

I also suspected that if the password was set in the values.yaml file, Helm could have created a Kubernetes secret for the Etcd server. To verify this, I listed the secrets in the cluster and retrieved the Etcd secret with the following commands:

    kubectl get secrets
    kubectl get secret/etcd-deployment -o yaml

I decoded the secret using an online base64 decoding tool, and found that both the decoded secret and the password in values.yaml were the same.


Remember: etcdctl utitly was already available/installed in etcd-server
