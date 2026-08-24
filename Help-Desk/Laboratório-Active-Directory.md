# Laboratório prático de Active Directory no Windows Server

Este repositório documenta a criação e administração de um ambiente de domínio fictício (**lab.local**) no Windows Server para simulação de chamados de Suporte N1.

---

## Tecnologias e Ferramentas Utilizadas
* **Virtualização:** Oracle VirtualBox
* **Sistema Operacional:** Windows Server Desktop Experience
* **Funções de Rede:** Active Directory Domain Services (**AD DS**)
* **Ferramentas de Gestão:** Active Directory Users and Computers (**dsa.msc**) e Prompt de Comando (**cmd**)

---

## 1. Estrutura de Unidades Organizacionais (OUs)
A estrutura foi criada para simular  um ambiente corporativo organizado por setores como um escritório com computadores (Computers), grupos (Groups) e usuários (Users) como o TI (IT) e o RH (HR):

![Estrutura de Unidades Organizacionais](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-Unidades-Organizacionais.png)

---

## 2. Simulações de rotinas de Suporte N1

### Caso 1: Provisionamento de Novo Colaborador
* **Cenário:** Admissão de usuário John Smith no setor de TI.

* **Ação:** Criação do usuário **john.smith**.

* **Política de Segurança:** Seleção da flag (**User must change password at next logon**) em conformidade com as diretrizes de privacidade e segurança da informação, garantindo que o suporte não tenha conhecimento da senha definitiva definida posteriormente pelo usuário.

  ![Provisionamento de Novo Colaborador](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-User-1.png)

  ![Provisionamento de Novo Colaborador](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-User-2.png)

  

### Caso 2: Reset de Senha e Desbloqueio
* **Cenário:** Colaborador bloqueou a conta por limite de tentativas de senha incorreta.

* **Ação:** Localização do objeto na OU correspondente, redefinição de credencial via menu de contexto e execução da flag **Unlock Account** para o devido desbloqueio do usuário.

* **Política de Segurança:** Seleção da flag (**User must change password at next logon**) em conformidade com as diretrizes de privacidade e segurança da informação, garantindo que o suporte não tenha conhecimento da senha definitiva definida posteriormente pelo usuário.

  ![Reset de Senha e Desbloqueio](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-Senha.png)

  

### Caso 3: Gestão de Acessos por Grupos de Segurança
* **Cenário:** Liberação de acesso a pastas de rede restritas.

* **Ação:** Criação do grupo de segurança **GRP_IT_Access** aplicando a atribuição de permissões via funcionalidade ***Member Of***.

  ![Gestão de Acessos por Grupos de Segurança](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-Acesso-1.png)

  ![Gestão de Acessos por Grupos de Segurança](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-Acesso-2.png)

---

## 3. Comandos de Diagnóstico Utilizados via CMD
* **gpupdate /force** - Força a atualização imediata das Diretivas de Grupo na máquina.

  ![gpupdate /force](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-CMD-gpupdate-force.png)

* **whoami /groups** - Lista os grupos de segurança do usuário ativo no prompt.

  ![whoami /groups](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-CMD-whoami-groups.png)

* **net user <usuario> /domain** - Consulta rápida dos atributos de conta no domínio.

![net user <usuario> /domain](https://github.com/bruno-cotoski/lab/blob/main/Help-Desk/pictures/Active-Directory-CMD-net-user-usuario-domain.png)
