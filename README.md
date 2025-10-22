# CI/CD - Despliegue de RBAC en OpenShift

Este repositorio automatiza el despliegue de configuraciones de seguridad (**ServiceAccount** y **RoleBinding**) en OpenShift mediante **GitHub Actions**.

---

## 📘 Descripción

El manifiesto [`ocsecurity.yaml`](./ocsecurity.yaml) define los permisos necesarios para operaciones de CI/CD dentro del namespace:

- **ServiceAccount:** `cicd-serviceaccount`  
- **RoleBinding:** `cicd-binding` con rol `edit`  
- **Namespace:** `ecuaalejo2013-dev`

El flujo de automatización se ejecuta al detectar cambios en el manifiesto o de forma manual desde GitHub Actions.

---

## ⚙️ Pipeline

El workflow [`main.yml`](.github/workflows/main.yml):

1. Clona el repositorio.  
2. Inicia sesión en OpenShift usando credenciales seguras.  
3. Valida y aplica `ocsecurity.yaml`.  
4. Muestra los recursos desplegados.

---

## 🔐 Configuración requerida

Agregar los siguientes **Secrets** en  
`Settings → Secrets and variables → Actions`:

| Variable | Descripción |
|-----------|--------------|
| `OPENSHIFT_SERVER` | URL del API Server (ej. `https://api.cluster.local:6443`) |
| `OPENSHIFT_TOKEN` | Token con permisos para crear recursos RBAC |
| `OPENSHIFT_NAMESPACE` | Namespace de destino (ej. `ecuaalejo2013-dev`) |

---

## 🧩 Uso

El pipeline se ejecuta automáticamente con cada *push* a `main` que modifique `ocsecurity.yaml`.  
También puede iniciarse manualmente desde la pestaña **Actions → Deploy RBAC (ocsecurity.yaml)**.

Para ejecución manual vía CLI:

```bash
oc login https://api.cluster.local:6443 --token=<token>
oc project ecuaalejo2013-dev
oc apply -f ocsecurity.yaml