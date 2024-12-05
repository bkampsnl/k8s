# Install

minikube addons enable ingress

# Argocd
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
brew install argocd
kubectl apply -f argocd-ingress.yaml -n argocd
echo "$(minikube ip) argocd.local" | sudo tee -a /etc/hosts
argocd admin initial-password -n argocd

# AWX
helm repo add awx-operator https://ansible-community.github.io/awx-operator-helm/
helm install awx-operator awx-operator/awx-operator -n awx --create-namespace
kubectl apply -f awx.yaml -n awx
kubectl apply -f awx-ingress.yaml -n awx
echo "$(minikube ip) awx.local" | sudo tee -a /etc/hosts

# Backstage
helm repo add backstage https://backstage.github.io/charts
kubectl create namespace backstage
helm upgrade -i backstage backstage/backstage -n backstage
echo "$(minikube ip) backstage.local" | sudo tee -a /etc/hosts
