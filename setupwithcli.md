
# Create a project
oc new-project test
oc label namespace/test openshift.io/cluster-monitoring=true


# Create a rules file locally
cat <<EOF >>rules.yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: example
  namespace: test
spec: 
  groups:
  - name: custom_rules
    rules:
    - expr: sum by (pod) (container_memory_working_set_bytes)
      record: memory_usage_by_pods
EOF

# Go to the projet
oc project test
oc create -f rules.yaml
oc label prometheusrule.monitoring.coreos.com/example prometheus=k8s role=alert-rules



