# **Docker Swarm: Monitoramento e observabilidade**

Este repositório fornece um stack completa para monitoramento de clusters Docker Swarm, integrando ferramentas como **Traefik**, **Prometheus**, **Grafana**, **Loki**, **Promtail**, **Node Exporter** e **cAdvisor**. Ele permite centralizar métricas, logs e dashboards para garantir visibilidade e monitoramento eficiente do cluster.

## 🚀**Visão Geral**

#### **Objetivo**
O stack foi projetado para monitorar clusters Docker Swarm, oferecendo:
- **Métricas detalhadas** de sistema, containers e serviços.
- **Centralização de logs** para fácil análise.
- **Dashboards interativos** com alertas configuráveis.
- **Escalabilidade** e integração nativa com o Docker Swarm.

#### **Principais Componentes**
1. **Traefik**: Proxy reverso e balanceador de carga.
2. **Prometheus**: Coleta e armazena métricas.
3. **Grafana**: Visualização de métricas e criação de dashboards.
4. **Loki & Promtail**: Sistema centralizado de logs.
5. **Node Exporter**: Métricas de sistema.
6. **cAdvisor**: Monitoramento de containers.

## 🌟**Recursos**

- Monitoramento em tempo real para todos os node e serviços do Swarm.
- Centralização de logs com suporte a consultas.
- Implantação automática em nodes específicos via labels.
- Suporte a armazenamento persistente para dados e logs.
- Dashboards pré-configurados no Grafana.

## ⚙️**Guia de Implantação**

### **Pré-requisitos**
1. **Cluster Docker Swarm** já inicializado.
2. Rede externa chamada `traefik-net`:
   ```bash
   docker network create --driver=overlay traefik-net
   ```
3. Labels configuradas nos node onde os serviços serão executados:
   ```bash
   docker node update --label-add monitoring=true <nome_do_no>
   ```

### **Etapas**

1. Clone este repositório:
   ```bash
   git clone https://github.com/lucasaffonso0/monitoring-docker-swarm.git
   cd monitoring-docker-swarm
   ```
2. Substitua `seudominio.com` pelo seu domínio no arquivo docker-compose.yml com o comando abaixo:
```sed -i "s/seudominio.com/seu-dominio-aqui.com/g" docker-compose.yml```

3. Configure o usuário e senha para acessar o grafana. Para isso precisaremos criar duas secrets, usando o comando abaixo:
```
echo "seu-usuario" | docker secret create grafana_admin_user -
echo "sua-senha" | docker secret create grafana_admin_password -
```
4. Realize o deploy da stack:
   ```bash
   docker stack deploy -c docker-compose.yml traefik
   ```
5. Acesse os serviços:
   - **Prometheus**: `http://prometheus.seu-dominio-aqui.com`
   - **Grafana**: `http://monitoring.seu-dominio-aqui.com`
   - **Loki**: `http://loki.seu-dominio-aqui.com`

## 📂**Detalhes dos Serviços**

### **Traefik**
- Proxy reverso e expor métricas do mesmo em `/metrics`.
- Configurado para integrar com Swarm e expor serviços de forma automática.

### **Prometheus**
- Coleta métricas de:
  - Node Exporter (node).
  - cAdvisor (containers).
  - Traefik (tráfego e status).
- Exposto na porta `9090`.

### **Grafana**
- Dashboards interativos para visualização de métricas.
- Recomendação de dashboards:
    - https://grafana.com/grafana/dashboards/1860-node-exporter-full/
    - https://grafana.com/grafana/dashboards/19908-docker-container-monitoring-with-prometheus-and-cadvisor/
    - https://grafana.com/grafana/dashboards/13946-docker-cadvisor/
    - https://grafana.com/grafana/dashboards/14282-cadvisor-exporter/

### **Loki & Promtail**
- **Loki** armazena logs.
- **Promtail** coleta logs dos containers e envia para o Loki.

### **Node Exporter**
- Exporta métricas de sistema, como uso de CPU, memória e disco.

### **cAdvisor**
- Monitora o desempenho e consumo de recursos de containers.

## **Armazenamento e Rede**

### **Rede**
- Rede externa `traefik-net` para comunicação entre serviços.

### **Volumes Persistentes**
- **prometheus_data**: Armazena dados de métricas.
- **grafana_data**: Persiste configurações e dashboards do Grafana.
- **loki_data**: Armazena logs processados pelo Loki.

### **Secrets**
- **prometheus_config**: Configuração do Prometheus.
- **loki_config**: Configuração do Loki.
- **promtail_config**: Configuração do Promtail.

## 🤝**Contribuições**

Contribuições são bem-vindas! Abra uma issue ou envie um pull request com melhorias, correções ou novas funcionalidades.