
kubectl:
  FROM bitnami/minideb
  USER 0
  RUN install_packages curl ca-certificates && curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
  install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && rm -rf kubectl
  # wait-for-ssh
  RUN curl -LO https://raw.githubusercontent.com/groundnuty/k8s-wait-for/master/wait_for.sh && \
  install -o root -g root -m 0755 wait_for.sh /usr/local/bin/k8s_wait_for && rm -rf wait_for.sh
  # scripts
  WORKDIR /
  COPY scripts scripts
  RUN for script in /scripts/* ; do \
  sc="$(basename -- $script)" \
  install -o root -g root -m 0755 $script /usr/local/bin/$sc ; done \
  && rm -rf scripts
   
helm:
  FROM +kubectl
  RUN curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

build:
  FROM +helm 
  SAVE IMAGE --push heiran/kubectl

