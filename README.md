# Projeto 01 - DevOps com Vagrant e Ansible 

## 1. Informações do Projeto

* **Instituição:** IFPB Campus João Pessoa
* **Curso:** Redes de Computadores
* **Professor:** Leonidas Francisco de Lima Júnior
* **Disciplina:** Adminstração de Sistemas Abertos
* **Período:** 2025.1 
* **Equipe:**
    * Nilson Vinícius Aurelio Chaves 
    * Wellington Antonio da Silva 

## 2. Objetivo do Projeto

O objetivo principal deste projeto é desenvolver competências práticas em DevOps e Infraestrutura como Código (IaC) utilizando as ferramentas Vagrant e Ansible. O projeto visa provisionar uma infraestrutura virtual e automatizar a configuração de sistemas operacionais e serviços essenciais, simulando um ambiente de rede real. 

## 3. Escopo da Infraestrutura

O projeto provisiona e configura quatro máquinas virtuais (VMs) baseadas em Debian Bookworm, cada uma com um papel específico na rede:

* **Servidor de Arquivos (arq)**: Atua como servidor DHCP, NFS e gerenciador de volumes lógicos (LVM).
* **Servidor de Banco de Dados (db)**: Hospeda o MariaDB e monta compartilhamentos NFS via Autofs.
* **Servidor de Aplicação (app)**: Hospeda um servidor web Apache e monta compartilhamentos NFS via Autofs.
* **Host Cliente (cli)**: Simula um cliente de rede, com navegador Firefox-ESR, suporte a X11 Forwarding e montagem NFS via Autofs.

### Detalhes das VMs:

Todas as VMs utilizam:
* **Provedor:** VirtualBox 
* **Box Base:** `debian/bookworm64` 
* **Geração de Chaves SSH:** Desativada (gerenciada pelo Ansible) 
* *Tipo de Clone:** `linked_clone` (otimização de espaço) 
* **Verificação de Guest Additions:** Desabilitada 

**Configurações Específicas:**

* **arq (Servidor de Arquivos):**
    * **Hostname:** `arq.nilson.wellington.devops` 
    * **IP Privado:** `192.168.56.102` (IP estático) 
    * **Memória RAM:** 512 MB 
    * **Discos Adicionais:** 3 discos de 10 GB cada, utilizados para LVM 
* **db (Servidor de Banco de Dados):**
    * **Hostname:** `db.nilson.wellington.devops` 
    * **IP Privado:** Obtido via DHCP (IP fixo pelo servidor `arq`, ex: `192.168.56.131`) 
    * **Memória RAM:** 512 MB 
* **app (Servidor de Aplicação):**
    * **Hostname:** `app.nilson.wellington.devops` 
    * **IP Privado:** Obtido via DHCP (IP fixo pelo servidor `arq`, ex: `192.168.56.121`) 
    * **Memória RAM:** 512 MB 
* **cli (Host Cliente):**
    * **Hostname:** `cli.nilson.wellington.devops` 
    * **IP Privado:** Obtido via DHCP (IP dinâmico na faixa `192.168.56.50-100`) 
    * **Memória RAM:** 1024 MB 

## 4. Tecnologias Utilizadas

* **Vagrant:** Ferramenta para construção e gerenciamento de ambientes de máquinas virtuais.
* **VirtualBox:** Provedor de virtualização para as máquinas.
* **Ansible:** Ferramenta de automação para configuração de infraestrutura e aplicação de software.
* **Linux (Debian Bookworm):** Sistema operacional base das VMs.
* **DHCP:** Serviço de atribuição dinâmica e estática de IPs.
* **LVM:** Gerenciamento de volumes lógicos para discos.
* **NFS:** Sistema de arquivos de rede para compartilhamento de dados.
* **Autofs:** Serviço de montagem automática de sistemas de arquivos.
* **MariaDB:** Servidor de banco de dados relacional.
* **Apache2:** Servidor web HTTP.
* **SSH:** Configuração avançada (autenticação por chave, desabilita root, banner).
* **NTP (Chrony):** Sincronização de tempo.
* **Git/GitHub:** Controle de versão e hospedagem do projeto.

## 5. Estrutura do Projeto

A estrutura de diretórios do projeto segue as boas práticas do Ansible para organização de roles e playbooks:
