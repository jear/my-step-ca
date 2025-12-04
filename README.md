# my-step-ca

```
https://smallstep.com/docs/step-ca/installation/#kubernetes
https://artifacthub.io/packages/helm/smallstep/step-certificates

helm repo add smallstep https://smallstep.github.io/helm-charts/
helm repo update

helm upgrade --install  step-certificates smallstep/step-certificates -n step-ca --create-namespace
Release "step-certificates" does not exist. Installing it now.
NAME: step-certificates
LAST DEPLOYED: Thu Dec  4 23:08:08 2025
NAMESPACE: step-ca
STATUS: deployed
REVISION: 1
NOTES:
Thanks for installing Step CA.

1. Get the PKI and Provisioner secrets running these commands:
   kubectl get -n step-ca -o jsonpath='{.data.password}' secret/step-certificates-ca-password | base64 --decode
   kubectl get -n step-ca -o jsonpath='{.data.password}' secret/step-certificates-provisioner-password | base64 --decode
2. Get the CA URL and the root certificate fingerprint running this command:
   kubectl -n step-ca logs job.batch/step-certificates

3. Delete the configuration job running this command:
   kubectl -n step-ca delete job.batch/step-certificates

```


```
step certificate create --profile root-ca "Example Root CA" root_ca.crt root_ca.key
step certificate create "Example Intermediate CA 1"     intermediate_ca.crt intermediate_ca.key     --profile intermediate-ca --ca ./root_ca.crt --ca-key ./root_ca.key

#step certificate create xxx.lysdemolab.fr xxx.lysdemolab.fr.crt xxx.lysdemolab.fr.key  --profile leaf --not-after=8760h  --ca ./intermediate_ca.crt --ca-key ./intermediate_ca.key --bundle
#step certificate verify xxx.lysdemolab.fr.crt --roots root_ca.crt

# When password is requested ( Thales CM )
step certificate create thales-cm-1.lysdemolab.fr thales-cm-1.lysdemolab.fr.crt thales-cm-1.lysdemolab.fr.key  --profile leaf --not-after=8760h  --ca ./intermediate_ca.crt --ca-key ./intermediate_ca.key --bundle
step certificate verify thales-cm-1.lysdemolab.fr.crt --roots root_ca.crt
# Concatenate the crt and key in a .pem file
# Upload new cert in Thales CM web interface and restart web service
# Repeat for other cluster members :
step certificate create thales-cm-2.lysdemolab.fr thales-cm-2.lysdemolab.fr.crt thales-cm-2.lysdemolab.fr.key  --profile leaf --not-after=8760h  --ca ./intermediate_ca.crt --ca-key ./intermediate_ca.key --bundle
step certificate verify thales-cm-2.lysdemolab.fr.crt --roots root_ca.crt
...

# Without password and 2048 bits (iLO)
step certificate create ilo-dl380-nvidia-a100.lysdemolab.fr ilo-dl380-nvidia-a100.lysdemolab.fr.crt ilo-dl380-nvidia-a100.lysdemolab.fr.key --profile leaf --not-after=8760h  --ca ./intermediate_ca.crt --ca-key ./intermediate_ca.key --bundle --no-password --insecure  --kty RSA --size 2048

```


- https://step-ca.lysdemolab.fr/acme/acme/directory

```
#log into step-ca
MY_HOST=morpheus
step certificate inspect $MY_HOST.lysdemolab.fr.crt

step ca renew  $MY_HOST.lysdemolab.fr.crt  $MY_HOST.lysdemolab.fr.key
✔ Would you like to overwrite morpheus.lysdemolab.fr.crt [y/n]: y
Your certificate has been saved in morpheus.lysdemolab.fr.crt.

scp $MY_HOST.lysdemolab.fr.crt $MY_HOST.lysdemolab.fr.key intermediate_ca.crt ubnt@$MY_HOST.lysdemolab.fr:/home/ubnt/
ubnt@morpheus.lysdemolab.fr's password:
morpheus.lysdemolab.fr.crt                                                                            100% 1779     1.7MB/s   00:00
morpheus.lysdemolab.fr.key                                                                            100% 1675     1.7MB/s   00:00
intermediate_ca.crt                                                                                   100%  745   996.4KB/s   00:00



```
