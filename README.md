# ansible-test


criar namespace aap
instalar operador ansible automation platform

rodar os seguintes yamls no ocp:
não esqueça de trocar o route_host: para o endereço do AAP 
por exemplo: aap.apps.ocp.com

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: controller-admin-password
  namespace: aap
stringData:
  password: "admin"

# spec de definição de um controller, necessária atenção para o parametro route_host
---
apiVersion: automationcontroller.ansible.com/v1beta1
kind: AutomationController
metadata:
  name: controller
  namespace: aap
spec:
  create_preload_data: true
  route_tls_termination_mechanism: Edge
  garbage_collect_secrets: false
  loadbalancer_port: 80
  image_pull_policy: IfNotPresent
  task_privileged: false
  projects_persistence: false
  replicas: 1
  admin_user: "admin"
  admin_email: "admin@email.com"
  admin_password_secret: controller-admin-password
  loadbalancer_protocol: http
  route_host: aap.apps.cluster-xxxxxx.xxxxx.ocp.com
  projects_use_existing_claim: _No_
```
