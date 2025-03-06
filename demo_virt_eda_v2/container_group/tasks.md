[Tasks]

oc apply -f containergroup-sa.yml
oc login ------
oc project aap-exec

# Create a one-year long-lived token
oc create token containergroup-service-account --duration=$((365*24))h > containergroup-sa.token
# Maybe this right?
oc extract secret/serving-cert --keys=tls.crt --to=- -n openshift-apiserver > containergroup-sa.ca.crt


Create an OCP API Token credential in AAP
Then create a ContainerGroup with the API url and token (cert only needed if you turn on Verify SSL)
MAKE SURE you customise the pod spec to change the namespace and the serviceAccountName (containergroup-service-account)




:: The below doesn't work because there is no token automatically created any more...
:: 
export SA_SECRET=$(oc get sa containergroup-service-account -o json | jq '.secrets[0].name' | tr -d '"')
oc get secret $(echo ${SA_SECRET}) -o json | jq '.data.".dockercfg"' | xargs | base64 --decode > containergroup-sa.token
