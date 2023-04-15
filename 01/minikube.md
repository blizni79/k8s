Docker zakładał, że wszystko dzieje się na jednej fizycznie maszynie (host) i jej systemie operacyjnym. W celu zbudowania skalowalnego rozwiązania i spełniającego zasadę wysokiej dostępności (HA) należało to rozwiązanie rozproszyć pomiędzy fizyczne maszyny. Ponadto trzeba było zbudować mechanizm zarządzania tak zbudowanym rozwiązaniem (klastrem) czyli zadbać o orkiestrację takich maszyn. W tym celu powstało nowe rozwiązanie Kubernetes, które miało na celu rozwiązanie tak postawionego problemu biznesowego.

Projekt Kubernetes (k8s) powstał w roku 2014 w firmie Google. Oficjalnie pierwsze wydanie wersji nastąpiło w roku 2015 i jest on licencjonowany na zasadach open-source. W chwili obecnej jest to najbardziej powszechnie używany mechanizm orkiestracji kontenerów. Na jego podbudowie powstało też klika projektów komercyjnych, które rozwijają koncepcje w nim zawarte np. Open Shift firmy Redhat. 

W celu lepszego zrozumienia jak działa Kubernetes dokonamy analizy poniższego schematu.

Zacznijmy od składowych komponentów i wyjaśnijmy ich rolę w tym schemacie:

Worker Node – węzeł odpowiedzialny za pracę w klastrze. Możemy utożsamiać go z jedną fizyczną lub wirtualną maszyną.
Master Node – węzeł odpowiedzialny za kierowanie pracą w klastrze. Możemy taki node utożsamiać z jedną fizyczną lub wirtualną maszyną.
Cloud Provider API – API wystawione przez twórcę chmury za pomocą , którego możemy zintegrować nasz klaster z rozwiązaniem chmurowym
Na worker node możemy wyróżnić następujące komponenty:

kube-proxy – odpowiedzialny za przekierowane ruchu sieciowego do odpowiedniego poda
kubelet – serwis sterujący pracą jednego węzła
Container Runtime – środowisko uruchomieniowe dla kontenerów. Uruchamia odpowiednie pody oraz zawarte w nich kontenery (pierwotnie ta rolę pełnił docker)
Na master node możemy wyróżnić następujące elementy

https://pl.asseco.com/files/public/blog-tech/2_rm_Rysunek2.png

API Server – końcówka komunikacji z innymi serwisami oraz wystawia API do sterowania tą pracą z zewnątrz klastra
Etcd - baza na, której zapisywany jest stan oraz ustawienia klastra
Scheduler – serwis odpowiedzialny za wykonanie zleconych zadań dla klastra
Controller Manager – główny serwis sterujący pracą klastra
Controller Cluster Manager – serwis służący do integracji klastra z rozwiązaniami chmurowymi
Do sterowania pracą klastra Kubernetesa używamy polecenia kubectl. Program ten łączy się bezpośrednio z API Server.

Na zakończenie zróbmy pewne podsumowanie. Świat tworzenia oprogramowania ulega ciągłej zmianie. By można było stosować nowe wzorce takie jak mikroserwisy potrzebujemy odpowiednich narzędzi do ich wdrażania. Jako rozwiązanie tak sformułowanej potrzeby biznesowej powstał Docker a potem jego idee rozwinął Kubernetes.

W następnej części pokażemy jak postawić prosty klaster Kubernetesa wielo-węzłowy na naszej maszynie lokalnej. Użyjemy w tym celu pełnej wirtualizacji opartej o Oracle Virtualbox oraz oprogramowania Vagrant. Wykonamy także pierwszą aplikacje w .NET Core i wdrożymy ją na naszym klastrze.




curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube




minikube start


kubectl version --client

cat $HOME/.kube/config

cp $HOME/.kube/config $HOME/.kube/config.backup.$(date +%Y-%m-%d.%H:%M:%S)


minikube dashboard
minikube dashboard -url


kubectl get po -A
minikube kubectl -- get po -A
alias kubectl="minikube kubectl --"
