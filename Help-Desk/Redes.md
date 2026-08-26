# 🛠️ Laboratório Prático: Resolução de Problemas de Rede no Windows (CMD)

**Autor:** Bruno Daniel Carvalho Cotoski  
**Objetivo:** Simular, diagnosticar e solucionar os problemas mais frequentes de conectividade de rede em ambientes corporativos e de Help Desk utilizando o Prompt de Comando (CMD) do Windows.

---

## 📋 Visão Geral dos Cenários

| Cenário | Sintoma Relatado | Causa Raiz | Comando Principal |
| :--- | :--- | :--- | :--- |
| **01** | Sem acesso à internet / IP inválido (APIPA) | Falha na atribuição via DHCP | `ipconfig /renew` |
| **02** | Programas conectam, mas sites não abrem | Falha / Cache corrompido de DNS | `ipconfig /flushdns` |
| **03** | Lentidão e interrupção na conexão local | Perda de pacotes na rede local / Gateway | `ping -t [IP]` |
| **04** | Conexão caindo no acesso a servidores externos | Falha de rota em nó intermediário | `tracert [Host]` |

---

## 🛠️ Cenários Práticos de Atendimento

### Cenário 1: Falha na Obtenção de IP (DHCP & APIPA)
**Sintoma:** O usuário informa que o computador está conectado via cabo, mas não consegue acessar nenhum recurso interno ou externo.

#### 1. Diagnóstico Inicial:
Ao rodar o comando `ipconfig`, verificou-se que a placa de rede recebeu um endereço no bloco `169.254.x.x` (APIPA), indicando que o servidor DHCP não respondeu a tempo.

```cmd
C:\Users\Bruno> ipconfig

Adaptador Ethernet Ethernet0:
   Sufixo DNS específico de conexão. . . : 
   Endereço IPv4 de Configuração Automática. . . : 169.254.45.12
   Máscara de Sub-rede . . . . . . . . . . . . . : 255.255.0.0
   Gateway Padrão. . . . . . . . . . . . . . . . : 
```

![Diagnóstico Cenário 1](img/cenario1-diagnostico.png)

#### 2. Resolução:
Executada a liberação do endereço IP temporário e solicitada uma nova concessão ao servidor DHCP da rede local.

```cmd
C:\Users\Bruno> ipconfig /release
C:\Users\Bruno> ipconfig /renew

Adaptador Ethernet Ethernet0:
   Endereço IPv4. . . . . . . . . . . . . . . . . : 192.168.1.105
   Máscara de Sub-rede . . . . . . . . . . . . . : 255.255.255.0
   Gateway Padrão. . . . . . . . . . . . . . . . : 192.168.1.1
```

![Solução Cenário 1](img/cenario1-solucao.png)

---

### Cenário 2: Falha de Resolução de Nomes (DNS Cache)
**Sintoma:** O usuário relata que consegue usar o aplicativo do Teams/Discord, mas os navegadores exibem a mensagem "Não foi possível encontrar o endereço IP do servidor".

#### 1. Diagnóstico Inicial:
Teste de conectividade por IP direto (ex: `8.8.8.8`) respondeu com sucesso, porém o teste por nome de domínio (ex: `google.com`) falhou, confirmando problema exclusivo na camada de resolução de nomes (DNS).

```cmd
C:\Users\Bruno> ping 8.8.8.8
Resposta de 8.8.8.8: bytes=32 tempo=15ms TTL=117

C:\Users\Bruno> ping google.com
A solicitação de ping não pôde encontrar o host google.com. Verifique o nome e tente novamente.
```

![Diagnóstico Cenário 2](img/cenario2-diagnostico.png)

#### 2. Resolução:
Limpeza do cache do resolvedor DNS local para eliminar registros corrompidos ou obsoletos.

```cmd
C:\Users\Bruno> ipconfig /flushdns

Configuração do IP do Windows
Liberação do Cache do Resolver do DNS bem-sucedida.
```

![Solução Cenário 2](img/cenario2-solucao.png)

---

### Cenário 3: Diagnóstico de Instabilidade e Perda de Pacotes (ICMP)
**Sintoma:** A aplicação interna da empresa desconecta intermitentemente ao longo do dia.

#### 1. Diagnóstico Inicial:
Disparo de teste contínuo de conectividade contra o Gateway Padrão (roteador da rede local) para identificar perdas na camada física ou sobrecarga de equipamentos.

```cmd
C:\Users\Bruno> ping -t 192.168.1.1

Resposta de 192.168.1.1: bytes=32 tempo=2ms TTL=64
Esgotado o tempo limite da solicitação.
Resposta de 192.168.1.1: bytes=32 tempo=150ms TTL=64
Resposta de 192.168.1.1: bytes=32 tempo=1ms TTL=64

Estatísticas do Ping para 192.168.1.1:
    Pacotes: Enviados = 10, Recebidos = 9, Perdidos = 1 (10% de perda)
```

![Diagnóstico Cenário 3](img/cenario3-diagnostico.png)

#### 2. Ação Corretiva:
Identificada oscilação física na porta de rede/cabo patch cord. Recomenda-se a troca do cabo e validação de porta no Switch de distribuição.

---

### Cenário 4: Rastreamento de Rota de Conexão (Tracert)
**Sintoma:** O usuário não consegue acessar o servidor em nuvem da empresa.

#### 1. Diagnóstico:
Mapeamento de todos os saltos (roteadores intermediários) percorridos pelos pacotes até o destino final.

```cmd
C:\Users\Bruno> tracert 1.1.1.1

Rastreando a rota para one.one.one.one [1.1.1.1]
com no máximo 30 saltos:

  1    1 ms    1 ms    1 ms  192.168.1.1
  2   10 ms    9 ms   12 ms  10.200.0.1
  3    *       *       *     Esgotado o tempo limite da solicitação.
  4    *       *       *     Esgotado o tempo limite da solicitação.
```

![Diagnóstico Cenário 4](img/cenario4-diagnostico.png)

#### 2. Análise de Suporte:
O rastreamento comprovou que a comunicação sai da rede local (salto 1 e 2), mas é bloqueada no link do provedor/link externo, descartando problema de hardware no computador do usuário final.

---

## 🚀 Conclusão e Aprendizados
* A utilização combinada de `ipconfig`, `ping` e `tracert` permite isolar falhas de rede em poucos minutos.
* Distinguir problemas de camadas (Física, IP e DNS) evita chamados desnecessários a fornecedores externos.
* A documentação sistemática de incidentes otimiza o tempo de atendimento em equipes de Help Desk N1/N2.
