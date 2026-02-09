# MySQL Service Operator

Ez az operátor egy **MySQL adatbázis-szolgáltatást** biztosít Kubernetes környezetben
deklaratív módon, **Custom Resource (CR)** segítségével.

A cél az, hogy egy alkalmazáscsapat **egyetlen YAML-lel** kérhessen:
- egy új MySQL példányt
- opcionális metrikagyűjtést
- elkülönített namespace-ben futó erőforrásokat
- automatikusan elérhető Grafana dashboardot

---

## Fő képességek

### ✅ MySQL példány létrehozása CR-rel
Egy `MySQLService` CR létrehozásával az operátor automatikusan létrehozza:
- `StatefulSet` (MySQL 8.0)
- perzisztens `PersistentVolumeClaim`
- `Service` (NodePort / ClusterIP)
- egyedi `Secret` a root jelszóval

Minden példány **név alapján izolált**, így több MySQL is futhat ugyanabban a clusterben,
akár azonos namespace-ben is.

---

### 🔐 Biztonságos jelszókezelés
- A MySQL root jelszó **automatikusan generálódik**
- A jelszó egy Kubernetes `Secret`-be kerül
- Új reconcile esetén **nem változik meg**
- A Secret az **egyetlen igazságforrás**

---

### 📊 Opcionális metrikagyűjtés (mysqld-exporter)
A CR `spec.enabledMetrics` mezője határozza meg, hogy legyen-e metrikagyűjtés.

```yaml
spec:
  enabledMetrics: true
