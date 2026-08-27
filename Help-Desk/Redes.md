# 🪛 Guia Prático de Diagnóstico de Redes no Windows 

**Objetivo:** Documentação de estudo dos comandos essenciais de linha de comando (CLI) no Windows CMD para diagnóstico e resolução de problemas de conectividade.

---

## 📌 Comandos Essenciais e Aplicação Prática

### 1. Identificação de Endereçamento IP (`ipconfig`)
Utilizado para verificar a configuração em tempo real da placa de rede, verificando se o dispositivo recebeu um IP válido pelo DHCP e identificando o IP do Gateway.

```cmd
ipconfig
```

![ipconfig](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-01-ipconfig.png)

### 2. Teste de Conectividade Básica (`ping`)

Disparo de pacotes ICMP para testar se existe comunicação física com um servidor externo direto pelo IP, desconsiderando problemas de resolução de nomes.

```cmd
ping 8.8.8.8
```

![ping](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-02-ping-ip.png)

### 3. Validação do Serviço de DNS (`ping [domínio]`)

Utilizado para testar se o servidor DNS configurado está traduzindo a URL amigável em linguagem humana em endereço IP válido.

```cmd
ping google.com
```

![DNS](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-03-ping-dns.png)

### 4. Rastreamento de Saltos e Latência (`tracert`)

Mapeamento da rota total percorrida pelo pacote até o destino para identificar especificamente qual ponto/roteador ocorre lentidão ou interrupção de sinal.

```cmd
tracert 1.1.1.1
```

![tracert](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-04-tracert.png)

---

## 📌 Matriz de Resolução de Problemas (Troubleshooting)

### Caso 1: "A internet não funciona em nenhum site"

- **Diagnóstico:** Executa o `ipconfig` e observa o IP `169.254.X.X` (APIPA).
- **Causa:** O computador não conseguiu se comunicar com o servidor DHCP do roteador.
- **Ação do Suporte:** Reiniciar o adaptador de rede ou renovar o IP com os comandos `ipconfig /release` e `ipconfig /renew`.

### Caso 2: "O WhatsApp Web funciona, mas os sites não abrem no navegador"

- **Diagnóstico:** Executa o comando  `ping 8.8.8.8` (responde com sucesso) e em seguida `ping google.com` (falha na conexão).
- **Causa:** Falha de resolução de nomes (Servidor DNS fora do ar ou incorreto na máquina).
- **Ação do Suporte:** Alterar o DNS da placa de rede para o IP público do Google (`8.8.8.8`).

### Caso 3: "O sistema da empresa está muito lento hoje"

- **Diagnóstico:** Executa o `tracert 1.1.1.1` e verifica o tempo de resposta elevado nos primeiros saltos.
- **Causa:** Congestionamento no roteador local ou na rede da operadora.
- **Ação do Suporte:** Identificar em qual salto ocorreu a oscilação de latência para isolar o problema identificando se é falha interna ou da operadora de internet.

## 💡 Conclusão

Este fluxo permite isolar falhas de rede no atendimento ao cliente com maior eficiência de tempo:

1. `ipconfig` valida a placa local.
2. `ping [IP]` valida o link físico e internet.
3. `ping [Domínio]` valida o serviço de DNS.
4. `tracert` isola falhas em rotas externas.
