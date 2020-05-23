# octo-pfe-helm-chart

# Execution on minikube

1- Add the following lines to ```/etc/hosts```, replace the IP with your minikube IP (```mikinube ip```):
```
10.0.2.15 keycloak.octo.com
10.0.2.15 react.octo.com
10.0.2.15 spring.octo.com
```

2- Modify Coredns ConfigMap to solve invalid issuer error of Keycloak:
Execute this command: kubectl replace -n kube-system -f coredns.yaml
Or just edit the ConfigMap directly: ```kubectl edit -n kube-system configmap coredns```
Add this line to Corefile data: ```rewrite name keycloak.octo.com bank-keycloak-http.default.svc.cluster.local```

3- Create realm secret:
```
kubectl create secret generic realm-secret --from-file=realm.json
```

4- Install release :
```
Helm install bank .
```

5- Open keycloak.octo.com in the browser, login with credentials keycloak/keycloak (If you've kept the defaults in values.yaml) then add a user. \

6- Open react.octo.com and enter user credentials.

# Important
For the moment you should use release name ```bank```, if you decide to use another, make sure to edit the line added in step 2 with ```rewrite name keycloak.octo.com <MY_RELEASE_NAME>-keycloak-http.default.svc.cluster.local```.
And modify ```dbHost``` in ```values.yaml``` to ```<MY_RELEASE_NAME>-postgresql```
(This requirement is needed because the current Keycloak chart doesn't allow to passing  {{ .Release.Name }} from values.yaml)

# TODO
- Add default some default users to Keycloak.
- Add bash script to automate the above steps 1 to 4.
