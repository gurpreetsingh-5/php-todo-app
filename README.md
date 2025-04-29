# php-todo-app
## Output
![image](https://github.com/user-attachments/assets/6c5b5464-d7b8-446e-afcb-f25804bcff69)
<table>
  <tr>
    <td>
      <a href="https://youtu.be/jehqpSE6gBE?si=QIgqcP6f_OEdUNZs" target="_blank">
        <img src="https://ytcards.demolab.com/?id=jehqpSE6gBE&title=Part+-+19.+Deploying+PHP+Todo+App+on+Kubernetes+with+MySQL+and+phpMyAdmin+Configuration+%7C+%40SenDevOps&lang=en&timestamp=0&background_color=%230d1117&title_color=%23ffffff&stats_color=%23dedede&max_title_lines=1&width=250&border_radius=5" 
        alt="Part - 19. Deploying PHP Todo App on Kubernetes with MySQL and phpMyAdmin Configuration | @SenDevOps" width="250" style="border-radius:5px;">
      </a>
    </td>
    <td>
      <a href="https://youtu.be/Ra36g3BBT_I?si=TYIYLHX53eke_J7w" target="_blank">
        <img src="https://ytcards.demolab.com/?id=Ra36g3BBT_I&title=Part+-+20.+Deploy+3+Tier+PHP+Application+on+Kubernetes+Using+ArgoCD+%7C+%40SenDevOps+%23devops&lang=en&timestamp=0&background_color=%230d1117&title_color=%23ffffff&stats_color=%23dedede&max_title_lines=1&width=250&border_radius=5" 
        alt="Part - 20. Deploy 3 Tier PHP Application on Kubernetes Using ArgoCD | @SenDevOps #devops" width="250" style="border-radius:5px;">
      </a>
    </td>
    <td>
      <a href="https://youtu.be/LD7CVc_CyGE?si=T3xXtJpW6tnRnG6F" target="_blank">
        <img src="https://ytcards.demolab.com/?id=LD7CVc_CyGE&title=Part+-+21.+PHP+3+Tier+Application+Deploy+on+Kubernetes+%7C+Host+Problem+Solved+%7C+%40SenDevOps+%23devops&lang=en&timestamp=0&background_color=%230d1117&title_color=%23ffffff&stats_color=%23dedede&max_title_lines=1&width=250&border_radius=5" 
        alt="Part - 21. PHP 3 Tier Application Deploy on Kubernetes | Host Problem Solved | @SenDevOps #devops" width="250" style="border-radius:5px;">
      </a>
    </td>
  </tr>
</table>


# Get DNS of a Service Programmatically
```
SERVICE_NAME=$(kubectl get svc mysql-service -o jsonpath='{.metadata.name}')
NAMESPACE=$(kubectl get svc mysql-service -o jsonpath='{.metadata.namespace}')
DNS="$SERVICE_NAME.$NAMESPACE.svc.cluster.local"
echo $DNS
```
## DNS format for a service
```
<service-name>.<namespace>.svc.cluster.local
```
# Mysql Pod
- db-cm
```
kubectl create cm db-cm --from-literal=MYSQL_DATABASE=mydb --dry-run=client -o yaml > db-cm.yaml
```
- db-secret

```
kubectl create secret generic db-secret --from-literal=MYSQL_ROOT_PASSWORD=rootpassword --dry-run=client -o yaml > db-secret.yaml
```
- mysql-pod
```
kubectl run mysql-pod --image=mysql --dry-run=client -o yaml > mysql-pod.yaml
```

- expose mysql pod service
```
kubectl expose pod mysql-pod --port=3306 --target-port=3306 --name=mysql-svc --dry-run=client -o yaml > mysql-svc.yaml
```

# Phpmyadmin Pod
- phpadmin-cm
```
kubectl create cm phpadmin-cm --from-literal=PMA_HOST=4.5.5.5 --from-literal=PMA_PORT=3306 --dry-run=client -o yaml > phpadmin-cm.yaml
```
- phpadmin-secret

```
kubectl create secret generic phpadmin-secret --from-literal=PMA_USER=root --from-literal=PMA_PASSWORD=rootpassword --dry-run=client -o yaml > phpadmin-secret.yaml
```
- phpadmin-pod
```
kubectl run phpadmin-pod --image=phpmyadmin --dry-run=client -o yaml > phpadmin-pod.yaml
```

- expose phpadmin pod service
```
kubectl expose pod phpadmin-pod --type=NodePort --port=8090 --target-port=80 --name=phpadmin-svc --dry-run=client -o yaml > phpadmin-svc.yaml
```
# Application Pod
- phpapp-pod
```
kubectl run phpapp-pod --image=fir3eye/php-app:v1 --dry-run=client -o yaml > phpapp-pod.yaml
```
- Expose pod service
```
kubectl expose pod phpapp-pod --type=NodePort --port=8020 --target-port=80 --name=phpapp-svc --dry-run=client -o yaml > phpapp-svc.yaml
```
