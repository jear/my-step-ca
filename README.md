# my-step-ca
```
step certificate create --profile root-ca "Example Root CA" root_ca.crt root_ca.key
step certificate create "Example Intermediate CA 1"     intermediate_ca.crt intermediate_ca.key     --profile intermediate-ca --ca ./root_ca.crt --ca-key ./root_ca.key

#step certificate create xxx.lysdemolab.fr xxx.lysdemolab.fr.crt xxx.lysdemolab.fr.key  --profile leaf --not-after=8760h  --ca ./intermediate_ca.crt --ca-key ./intermediate_ca.key --bundle
#step certificate verify xxx.lysdemolab.fr.crt --roots root_ca.crt

# When password is requested ( Thales CM )
step certificate create thales-cm-1.lysdemolab.fr thales-cm-1.lysdemolab.fr.crt thales-cm-1.lysdemolab.fr.key  --profile leaf --not-after=8760h  --ca ./intermediate_ca.crt --ca-key ./intermediate_ca.key --bundle
step certificate verify thales-cm-1.lysdemolab.fr.crt --roots root_ca.crt
# Concatenate the crt and key in the same file
# Upload new cert in Thales CM web interface
# Repeat for other cluster members


# Without password and 2048 bits (iLO)
step certificate create ilo-dl380-nvidia-a100.lysdemolab.fr ilo-dl380-nvidia-a100.lysdemolab.fr.crt ilo-dl380-nvidia-a100.lysdemolab.fr.key --profile leaf --not-after=8760h  --ca ./intermediate_ca.crt --ca-key ./intermediate_ca.key --bundle --no-password --insecure  --kty RSA --size 2048

```
