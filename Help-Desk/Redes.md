# 🛠️ Guia Prático de Diagnóstico de Redes no Windows (Help Desk N1)

**Objetivo:** Documentação prática dos comandos essenciais de linha de comando (CLI) do Windows para diagnóstico e resolução de problemas de conectividade em ambientes de Suporte Técnico.

---

## 📌 Comandos Essenciais e Aplicação Prática

### 1. Identificação de Endereçamento IP (`ipconfig`)
Utilizado para verificar a configuração atual da placa de rede, garantindo que o dispositivo recebeu um IP válido via DHCP e identificando o IP do Roteador/Gateway.

```cmd
ipconfig
```

![ipconfig](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-01-ipconfig.png)

### 2. Teste de Conectividade Básica (`ping`)

Disparo de pacotes ICMP para testar se há comunicação física com um servidor externo direto via IP, descartando problemas de resolução de nomes.

```cmd
ping 8.8.8.8
```

![ping](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-02-ping-ip.png)

### 3. Validação do Serviço de DNS (`ping [domínio]`)

Utilizado para testar se o servidor DNS configurado está traduzindo URLs amigáveis em endereços IP válidos na rede.

```cmd
ping google.com
```

![DNS](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-03-ping-dns.png)

### 4. Rastreamento de Saltos e Latência (`tracert`)

Mapeamento de toda a rota percorrida pelo pacote até o destino para identificar exatamente em qual ponto/roteador ocorre lentidão ou interrupção de sinal.

```cmd
tracert 1.1.1.1
```

![tracert](https://github.com/bruno-cotoski/alquimia/blob/main/Help-Desk/pictures/Redes-04-tracert.png)

---

## 📌 Matriz de Resolução de Problemas (Troubleshooting N1)

### Caso 1: "A internet não funciona em nenhum site"

- **Diagnóstico:** Executa-se `ipconfig` e observa-se o IP `169.254.X.X` (APIPA).
- **Causa:** O computador não conseguiu comunicação com o servidor DHCP do roteador.
- **Ação do Suporte:** Reiniciar o adaptador de rede ou renovar o IP com os comandos `ipconfig /release` e `ipconfig /renew`.

### Caso 2: "O WhatsApp Web funciona, mas os sites não abrem no navegador"

- **Diagnóstico:** Executa-se `ping 8.8.8.8` (responde com sucesso) e em seguida `ping google.com` (falha na conexão).
- **Causa:** Falha de resolução de nomes (Servidor DNS fora do ar ou incorreto na máquina).
- **Ação do Suporte:** Alterar o DNS da placa de rede para o IP público do Google (`8.8.8.8`).

### Caso 3: "O sistema da empresa está muito lento hoje"

- **Diagnóstico:** Executa-se o `tracert 1.1.1.1` e nota-se tempo de resposta elevado nos primeiros saltos.
- **Causa:** Congestionamento no roteador local ou na rede da operadora.
- **Ação do Suporte:** Identificar em qual salto ocorreu a oscilação de latência para isolar se a falha é interna ou da operadora de internet.

## 💡 Conclusão

Este fluxo sequencial permite isolar falhas de rede no atendimento ao cliente em menos de 2 minutos:

1. `ipconfig` valida a placa local.
2. `ping [IP]` valida o link físico e internet.
3. `ping [Domínio]` valida o serviço de DNS.
4. `tracert` isola falhas em rotas externas.
