# cybersecurity-desafio-medusa-nmap
projeto prático utilizando Kali Linux 





# **🛡️ Relatório de Auditoria: Testes de Força Bruta com Medusa**

Este relatório documenta a simulação de ataques de força bruta realizados em ambiente controlado, utilizando o **Kali Linux** como estação de ataque e o **Metasploitable 2** como alvo vulnerável.

## **1\. Configuração do Ambiente**

* **Atacante:** Kali Linux (IP: `192.168.xxx.xxx`)  
* **Alvo:** Metasploitable 2 (IP: `192.168.xxx.xxx`)  
* **Ferramenta Principal:** Medusa v2.3 / nmap.  
* **Rede:** Host-Only (Isolada no QEMU/KVM)

## **2\. Fase de Reconhecimento (Nmap)**

Antes de iniciar os ataques com o Medusa, foi necessário identificar o alvo e os serviços ativos para validar os pontos de entrada.

### **Identificação do Alvo**

Para descobrir o IP da máquina vulnerável na rede interna:

* **Comando:** `sudo nmap -sn 192.168.xxx.xxx`  
* **Objetivo:** Realizar um *Ping Scan* para listar todos os dispositivos conectados sem escanear portas ainda.

### 

### 

### **Varredura de Serviços e Portas**

Após identificar o IP `192.168.xxx.xxx`, realizei a varredura para confirmar as vulnerabilidades:

* **Comando:** `nmap -sV 192.168.xxx.xxx`  
* **O que o comando faz:**  
  * `-sV`: Tenta determinar a versão do serviço rodando em cada porta aberta.  
* **Resultado:** Foram identificadas as portas **21 (FTP)**, **445 (SMB)** e **80 (HTTP)** como abertas e prontas para o teste de intrusão.

## **3\. Execução dos Testes e Explicação dos Comandos**

### **Teste 01: Força Bruta no Serviço FTP (Porta 21\)**

### O objetivo foi testar a resistência do serviço de transferência de arquivos contra tentativas de login automatizadas.

* **Comando:** `medusa -h 192.168.xxx.xxx -U usuarios.txt -P senhas.txt -M ftp`  
* **Parâmetros:**  
  * `-h`: Define o Host (IP do alvo).  
  * `-U`: Indica o arquivo com a lista de usuários.  
  * `-P`: Indica o arquivo com a lista de senhas.  
  * `-M ftp`: Carrega o módulo específico para o protocolo FTP.

### 

### 

### **Teste 02: Password Spraying no SMB (Porta 445\)**

Testamos uma única senha contra vários usuários para evitar o bloqueio de contas por excesso de tentativas em um único login.

* **Comando:** `medusa -h 192.168.xxx.xxx -U usuarios.txt -p "msfadmin" -M smbnt`  
* **Parâmetros:**  
  * `-p` (minúsculo): Define uma **única senha** específica.  
  * `-M sbt`: Utiliza o módulo para o protocolo SMB (Samba/Windows).

### **Teste 03: Brute Force em Formulário Web (HTTP)**

Simulação de um ataque contra a interface de login de uma aplicação web (DVWA).

* **Comando:** `medusa -h 192.168.xxx.xxx -u admin -P senhas.txt -M http -m FORM:"login.php"`  
* **Parâmetros:**  
  * `-u` (minúsculo): Foca em um único usuário conhecido (**admin**).  
  * `-m FORM:"login.php"`: Informa que o alvo é um formulário de login específico na página.

## **4\. Análise de Vulnerabilidades Encontradas**

Durante os testes, foi possível identificar que:

1. **Credenciais Fracas:** O sistema permitia o uso de usuários e senhas idênticos (ex: `msf admin` / `msfadmin`).  
2. **Ausência de Rate Limiting:** O serviço não bloqueou o IP do atacante mesmo após centenas de tentativas em poucos segundos.

## 

## 

## **5\. Recomendações de Mitigação**

Para proteger o ambiente contra esses ataques, recomenda-se:

* **Implementação de MFA:** Autenticação de dois fatores.  
* **Fail2Ban:** Monitorar logs e banir IPs que falharem no login repetidamente.  
* **Políticas de Senha:** Exigir senhas complexas que não constem em wordlists comuns.
