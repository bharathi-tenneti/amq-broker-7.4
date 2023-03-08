# Deploy amq-broker 7.4 on Redhat Openshift using template


This document lays out steps to deploy AMQ Broker 7.4 on Redhat Openshift 4.10 via template.

1 - Create keystore and truststore files using the certs.sh script, keep note of the password, as we will be using this as param for the template.

```
$ ./certs.sh

```

2 - Create Openshift project for AMQ broker

```
$ oc  new-project amq-broker

```

3 - Generate Openshift secret to store the truststore and keystore files created from above step.

```
$ oc create secret generic amq-app-secret --from-file=broker.ks --from-file=broker.ts

```

4 - Generate another secret to store credentials for the AMQ broker.

```

$ oc create secret generic amq-credential-secret --from-literal admin=admin --from-literal password=admin --from-literal  amq_truststore_password=<password_typed_to_create_certs> --from-literal amq_keystore_password=<password_typed_to_create_certs>

```

5 - Create template for AMQ broker

```
$ oc create -f <path-to-templateyaml>

```

6 - Create new AMQ Broker deployment using the template amq-broker-74-ssl.yaml

```

$ oc new-app --template=amq-broker-74-ssl \ 
 --param AMQ_KEYSTORE_PASSWORD=password \
 --param AMQ_TRUSTSTORE_PASSWORD=password \
 --param AMQ_USER=admin\
 --param AMQ_PASSWORD=admin \
 --param AMQ_SECRET=amq-app-secret \
 --param AMQ_TRUSTSTORE=broker.ts \
 --param AMQ_KEYSTORE=broker.ks \
 --param AMQ_REQUIRE_LOGIN=true


```


