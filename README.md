<div align="center">

# 📡 Zabbix: monitoramento open source, sem licença por dispositivo

**Observabilidade para TI e OT na sua infraestrutura, com seus dados, sua marca e o seu ritmo de evolução.**

[![Licença](https://img.shields.io/badge/licen%C3%A7a-AGPLv3-blue)](https://www.zabbix.com/br/license)
[![Self-hosted](https://img.shields.io/badge/self--hosted-sim-success)](#-in%C3%ADcio-r%C3%A1pido)
[![Zabbix](https://img.shields.io/badge/Zabbix-7.2.4-D40000)](#-in%C3%ADcio-r%C3%A1pido)
[![Debian](https://img.shields.io/badge/Debian-12-A81D33?logo=debian&logoColor=white)](#-in%C3%ADcio-r%C3%A1pido)
[![Docker](https://img.shields.io/badge/docker-compose-2496ED?logo=docker&logoColor=white)](#-docker)
[![Docs](https://img.shields.io/badge/docs-PT--BR-informational)](https://www.zabbix.com/documentation/current/pt/manual/)

[Documentação](https://www.zabbix.com/documentation/current/pt/manual/) · [Início rápido](#-in%C3%ADcio-r%C3%A1pido) · [Personalização](#-personaliza%C3%A7%C3%A3o) · [Comparativo](#-comparativo) · [FAQ](#-faq)

</div>

---

## Índice

- [📖 O que é](#-o-que-%C3%A9)
- [🎯 Por que usar no lugar de ferramentas de monitoramento pagas?](#-por-que-usar-no-lugar-de-ferramentas-de-monitoramento-pagas)
- [✨ Principais recursos](#-principais-recursos)
- [🧩 Arquitetura](#-arquitetura)
- [🚀 Início rápido](#-in%C3%ADcio-r%C3%A1pido)
- [🐳 Docker](#-docker)
- [🔧 Configuração básica](#-configura%C3%A7%C3%A3o-b%C3%A1sica)
- [📡 Instalação do agente](#-instala%C3%A7%C3%A3o-do-agente)
- [🎨 Personalização](#-personaliza%C3%A7%C3%A3o)
- [🔒 Segurança](#-seguran%C3%A7a)
- [📊 Comparativo](#-comparativo)
- [💼 Casos de uso](#-casos-de-uso)
- [📌 Roadmap do repositório](#-roadmap-do-reposit%C3%B3rio)
- [🤝 Como contribuir](#-como-contribuir)
- [❓ FAQ](#-faq)
- [📄 Licença e créditos](#-licen%C3%A7a-e-cr%C3%A9ditos)

---

## 📖 O que é

O **Zabbix** é uma plataforma de monitoramento open source criada e mantida pela Zabbix LLC, com sede na Letônia e escritórios em outros países, incluindo o Brasil. Ela coleta métricas de redes, servidores, nuvem, containers, VMware, Windows, IoT, logs, aplicações e bancos de dados, avalia problemas com gatilhos (triggers), notifica a equipe e exibe tudo em dashboards. Pode rodar na sua infraestrutura (on-premises), na nuvem pública de sua escolha ou como serviço gerenciado (Zabbix Cloud).

Segundo o site oficial, são mais de 20 anos de projeto e mais de 300 mil implantações em mais de 190 países.

| Você conhece como… | No Zabbix é… |
|---|---|
| Dispositivo / host monitorado | **Host**, com templates aplicados |
| Coletor local | **Zabbix agent / agent 2** |
| Coletor remoto ou de filial | **Zabbix proxy** |
| Métrica | **Item** |
| Alerta | **Trigger** (gatilho) → **Problema** → **Ação** |
| Painel | **Dashboard** com widgets |
| Integração | **Media types**, webhooks e API |

> **Em uma frase:** o Zabbix coloca monitoramento corporativo no seu servidor, sem cobrança de licença por dispositivo, com código aberto para auditar e adaptar.

---

## 🎯 Por que usar no lugar de ferramentas de monitoramento pagas?

Muitas soluções comerciais de monitoramento cobram por host, por métrica ou por volume de dados. O Zabbix segue outro modelo, e é nele que está a diferença:

1. **🛠️ Customização.** Templates, itens, gatilhos, dashboards, scripts, webhooks, API e módulos de frontend permitem moldar o Zabbix ao seu ambiente, e o código aberto permite ir além.
2. **💰 Custo previsível.** O site oficial destaca ausência de taxa de licença e de taxa por dispositivo; o que existe é suporte opcional por assinatura. Confira valores em [Preços](https://www.zabbix.com/br/subscriptions).
3. **🔐 Dados sob seu controle.** Na instalação própria, métricas e históricos ficam no seu banco de dados, o que ajuda em LGPD, compliance e ambientes regulados ou isolados.
4. **🔓 Sem lock-in.** Banco aberto (PostgreSQL ou MySQL), API documentada e templates exportáveis facilitam migrar ou integrar.
5. **🏢 Multiempresa e MSP.** Grupos de hosts, permissões por usuário e proxies por cliente ou filial atendem equipes de TI internas e provedores de serviço (veja [Zabbix para MSP](https://www.zabbix.com/br/msp)).

---

## ✨ Principais recursos

**Coleta**
- Agentes (passivo e ativo), SNMP, IPMI, JMX, HTTP, SSH, Telnet, monitoramento de logs e verificações customizadas.
- Descoberta automática (LLD) de interfaces, discos, serviços e mais.
- Monitoramento web e sintético (disponível a partir do 7.0).

**Análise e alertas**
- Triggers com expressões flexíveis, dependências e correlação.
- Ações e escalonamento por e-mail, mensageiros e webhooks (ex.: sistemas de chamados).

**Visualização**
- Dashboards, mapas de rede, gráficos, relatórios e serviços de negócio (SLA).

**Escala e extensibilidade**
- Proxies para sites remotos, alta disponibilidade (a partir do 6.0), API JSON-RPC, templates da comunidade e [integrações oficiais](https://www.zabbix.com/br/integrations).

**Implantação**
- Pacotes Linux (incluindo Debian), containers Docker, imagens para nuvens públicas e [Zabbix Cloud](https://www.zabbix.com/br/cloud).

---

## 🧩 Arquitetura

```text
 ┌──────────────┐   HTTPS    ┌─────────────────────────────────────────┐
 │  Navegador   │ ─────────► │  Frontend web (Nginx/Apache + PHP)      │
 └──────────────┘            └───────────────┬─────────────────────────┘
                                             │
                             ┌───────────────▼───────────────┐
                             │  Zabbix Server  ◄──► Banco    │
                             │  (porta 10051)     PostgreSQL │
                             └───────┬───────────────┬───────┘   ou MySQL
                                     │               │
                    coleta direta    │               │  coleta via proxy
                                     ▼               ▼
                           ┌──────────────┐   ┌──────────────┐
                           │ Agent 2      │   │ Zabbix Proxy │ (filiais, DMZ)
                           │ (10050)      │   └──────┬───────┘
                           └──────────────┘          ▼
                                              Agents, SNMP, etc.
```

Com o **agente ativo**, o próprio agente inicia a conexão com o servidor ou proxy, o que facilita monitorar equipamentos atrás de NAT ou firewall (basta liberar a saída para a porta do servidor/proxy).

---

## 🚀 Início rápido

Guia de referência para **Debian 12 (Bookworm)** com **Zabbix 7.2.4**, **PostgreSQL + Nginx + Agent 2**.

> ⚠️ **Sobre a versão 7.2.** A 7.2 é uma versão padrão (não LTS) e, segundo a [política de ciclo de vida](https://www.zabbix.com/br/life_cycle_and_release_policy), chegou ao fim do suporte em **31/12/2025**, sem novas correções de segurança. A 7.2.4 também não é a última da série. Use-a para laboratório, compatibilidade com um ambiente existente ou estudo. Para **produção**, prefira uma versão **LTS** suportada (hoje a 7.0 LTS) e, se ficar na 7.2, planeje a migração. Os passos abaixo valem para outras versões trocando o número `7.2` no endereço do repositório.

Os comandos exatos por versão e sistema ficam no [seletor oficial de downloads](https://www.zabbix.com/br/download) (escolha *Debian 12*, banco PostgreSQL e servidor Nginx).

**Pré-requisitos:** servidor Debian dedicado (recomendado ao menos 2 vCPU e 4 GB de RAM para produção), acesso `root` ou `sudo`, e portas 80/443 (web) e 10051 (servidor) liberadas conforme a sua rede.

> Em instalações mínimas do Debian o `sudo` pode não estar instalado. Nesse caso, entre como `root` (`su -`) e remova o `sudo` dos comandos.

### 1. Instalar o PostgreSQL

🖥️ **No servidor Zabbix:**

```bash
sudo apt update
sudo apt install -y postgresql
```

### 2. Adicionar o repositório do Zabbix

🖥️ **No servidor Zabbix:**

```bash
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
sudo dpkg -i zabbix-release_latest_7.2+debian12_all.deb
sudo apt update
```

> Se o `wget` falhar, copie o link atualizado na [página de downloads](https://www.zabbix.com/br/download). O repositório pode ter sido reorganizado depois do fim do suporte da série.

### 3. Instalar servidor, frontend e agente (fixando a 7.2.4)

O repositório entrega por padrão a versão mais recente da série. Para ficar exatamente na **7.2.4**, liste as versões disponíveis e informe a desejada.

🖥️ **No servidor Zabbix:**

```bash
# Lista as versões disponíveis; procure a 7.2.4
apt-cache madison zabbix-server-pgsql
```

Instalação sem fixar a versão (instala a mais recente da série 7.2):

```bash
sudo apt install -y zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql \
                    zabbix-nginx-conf zabbix-sql-scripts zabbix-agent2
```

Para a **7.2.4**, acrescente `=VERSAO_EXATA` (copie a string da saída do `madison`, algo como `1:7.2.4-1+debian12`) a cada pacote Zabbix e depois impeça atualizações acidentais:

```bash
sudo apt install -y zabbix-server-pgsql=VERSAO_EXATA zabbix-frontend-php=VERSAO_EXATA \
                    zabbix-nginx-conf=VERSAO_EXATA zabbix-sql-scripts=VERSAO_EXATA \
                    zabbix-agent2=VERSAO_EXATA php8.2-pgsql
sudo apt-mark hold zabbix-server-pgsql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent2
```

> Se a 7.2.4 não aparecer na lista, o repositório não a oferece mais; nesse caso, será instalada a mais recente da série ou será preciso obter o pacote por outro meio.

### 4. Criar o banco de dados

🖥️ **No servidor Zabbix:**

```bash
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```

### 5. Configurar o servidor

🖥️ **No servidor Zabbix**, edite `/etc/zabbix/zabbix_server.conf`:

```ini
DBName=zabbix
DBUser=zabbix
DBPassword=SUA_SENHA_FORTE
```

### 6. Configurar o Nginx

🖥️ **No servidor Zabbix**, edite `/etc/zabbix/nginx.conf`, descomente e ajuste `listen` e `server_name`:

```nginx
listen          80;
server_name     zabbix.seudominio.com.br;
```

Se a porta 80 já estiver em uso pelo site padrão do Nginx, desative-o ou use outra porta (ex.: `8080`):

```bash
sudo rm -f /etc/nginx/sites-enabled/default
```

### 7. Gerar o locale pt_BR (para a interface em português)

🖥️ **No servidor Zabbix:**

```bash
sudo dpkg-reconfigure locales   # marque pt_BR.UTF-8 UTF-8
```

### 8. Iniciar os serviços

🖥️ **No servidor Zabbix** (no Debian 12 o PHP-FPM é `php8.2-fpm`):

```bash
sudo systemctl restart zabbix-server zabbix-agent2 nginx php8.2-fpm
sudo systemctl enable zabbix-server zabbix-agent2 nginx php8.2-fpm
sudo systemctl status zabbix-server
```

### 9. Primeiro acesso

🌐 **No navegador**, abra `http://zabbix.seudominio.com.br/` (ou o IP do servidor) e conclua o assistente do frontend, informando o banco `zabbix`, o usuário `zabbix` e a senha definida no passo 4. O login inicial padrão da documentação é `Admin` / `zabbix`: **troque a senha imediatamente** (veja [Segurança](#-seguran%C3%A7a)).

Se usar firewall (ex.: UFW), libere as portas necessárias:

🖥️ **No servidor Zabbix:**

```bash
sudo ufw allow 80,443/tcp
sudo ufw allow 10051/tcp
```

---

## 🐳 Docker

O projeto mantém imagens oficiais (`zabbix/zabbix-server-pgsql`, `zabbix/zabbix-web-nginx-pgsql`, `zabbix/zabbix-agent2`, entre outras) e um repositório com arquivos Compose prontos, o [zabbix-docker](https://github.com/zabbix/zabbix-docker). Esse é o caminho mais rápido para testar.

🖥️ **No servidor com Docker e Docker Compose instalados:**

```bash
git clone https://github.com/zabbix/zabbix-docker.git
cd zabbix-docker
# Opcional: use a branch/tag correspondente à versão desejada (ex.: série 7.2)
docker compose -f ./compose_pgsql.yaml up -d
docker compose -f ./compose_pgsql.yaml ps
```

- Os dados persistentes ficam no diretório `zbx_env/`, criado ao lado do arquivo Compose. **Inclua esse diretório no backup.**
- Variáveis (versão das imagens, portas, senhas do banco) ficam nos arquivos `.env` do repositório. Troque as senhas padrão antes de expor o serviço.
- Confira a porta publicada do frontend no próprio arquivo Compose e consulte a [documentação de instalação por containers](https://www.zabbix.com/documentation/current/pt/manual/installation/containers).

---

## 🔧 Configuração básica

### Servidor (`/etc/zabbix/zabbix_server.conf`)

| Parâmetro | Para que serve |
|---|---|
| `DBHost` | Servidor do banco (omita para socket local) |
| `DBName` | Nome do banco (`zabbix`) |
| `DBUser` | Usuário do banco |
| `DBPassword` | Senha do usuário do banco |

Demais parâmetros (cache, pollers, timeouts, TLS) estão descritos na [documentação do servidor](https://www.zabbix.com/documentation/current/pt/manual/). Ajuste só o que o seu volume exigir.

---

## 📡 Instalação do agente

O agente roda no equipamento a ser monitorado. O **Zabbix agent 2** é a opção recomendada para instalações novas.

### Linux (Debian)

🖥️ **No host Debian monitorado**, adicione o mesmo repositório do Zabbix (passo 2 do [Início rápido](#-in%C3%ADcio-r%C3%A1pido)) e instale o agente na versão compatível com o servidor (um agente da mesma série ou mais antigo que o servidor é o cenário mais seguro):

```bash
sudo apt install -y zabbix-agent2
```

Edite `/etc/zabbix/zabbix_agent2.conf`:

```ini
Server=IP_DO_SERVIDOR_OU_PROXY
ServerActive=IP_DO_SERVIDOR_OU_PROXY
Hostname=NOME_DO_HOST_IGUAL_AO_CADASTRADO_NO_ZABBIX
```

```bash
sudo systemctl enable --now zabbix-agent2
```

Se houver firewall no host monitorado (modo passivo), libere a porta do agente apenas para o servidor:

```bash
sudo ufw allow from IP_DO_SERVIDOR to any port 10050 proto tcp
```

### Windows

💻 **Na estação ou servidor Windows monitorado**, baixe o MSI do agent 2 na [página de downloads](https://www.zabbix.com/br/download) e execute, em um PowerShell como administrador:

```powershell
msiexec /i "zabbix_agent2-VERSAO-windows-amd64-openssl.msi" /qn SERVER=IP_DO_SERVIDOR SERVERACTIVE=IP_DO_SERVIDOR HOSTNAME=NOME_DO_HOST
```

### Cadastro no Zabbix

🌐 **No frontend web:** *Data collection → Hosts → Create host*, informe o mesmo `Hostname` do agente, o grupo e um template (ex.: Linux by Zabbix agent ou Windows by Zabbix agent).

| Modo | Quem inicia a conexão | Quando usar |
|---|---|---|
| Passivo | Servidor/proxy → agente (porta 10050) | Rede interna, sem restrições |
| Ativo | Agente → servidor/proxy (porta 10051) | Hosts atrás de NAT/firewall |

---

## 🎨 Personalização

Aqui o código aberto faz diferença. Os pontos abaixo são caminhos gerais; para detalhes e nomes exatos, siga a documentação oficial da sua versão.

| Item | Como |
|---|---|
| Monitoramento sob medida | Templates próprios, itens, gatilhos e macros |
| Dashboards | Widgets e dashboards por equipe ou cliente |
| Notificações | Media types e **webhooks** (chamados, mensageiros, automações) |
| Automação | **API JSON-RPC** para criar hosts, relatórios e integrações |
| Interface | Módulos de frontend e temas do Zabbix |
| Código | Fork do projeto, respeitando a licença AGPLv3 |

**Checklist de ideias para o seu projeto:**
- [ ] Biblioteca de templates padronizados para a sua operação
- [ ] Dashboards por cliente, filial ou setor
- [ ] Webhook para o seu sistema de chamados ou mensageiro
- [ ] Rotina via API para cadastrar hosts automaticamente
- [ ] Mapas de rede com a sua topologia

> ⚠️ **Marca e licença.** Se você pretende alterar identidade visual ou distribuir uma versão modificada, leia a [política de marcas registradas](https://www.zabbix.com/br/trademark) e a [licença](https://www.zabbix.com/br/license). Pela AGPLv3, quem modifica o código e o disponibiliza por rede ou distribuição deve compartilhar o código-fonte das modificações.

---

## 🔒 Segurança

- ✅ Troque a senha do usuário `Admin` no primeiro acesso e desative o usuário `guest`
- ✅ Publique o frontend apenas em **HTTPS**
- ✅ Ative autenticação em dois fatores para administradores
- ✅ Use criptografia (TLS/PSK) na comunicação entre agentes, proxies e servidor
- ✅ Restrinja portas (10050/10051) por firewall aos IPs necessários
- ✅ Use senhas fortes e exclusivas no banco de dados
- ✅ Mantenha o Zabbix em uma versão **com suporte** (a série 7.2 já encerrou o suporte) e acompanhe os [avisos de segurança](https://www.zabbix.com/br/security_advisories)
- ✅ Faça backup do banco e dos arquivos de configuração, e teste a restauração

---

## 📊 Comparativo

A comparação abaixo trata de **modelo e filosofia**, não de recursos ou preços de um produto específico. Preços, limites e funcionalidades de soluções comerciais mudam conforme o plano: confirme nos sites oficiais de cada fornecedor.

| Critério | Zabbix | Solução comercial típica |
|---|---|---|
| Código-fonte | Aberto (AGPLv3 a partir do 7.0) | Geralmente fechado |
| Licença | Sem taxa de licença nem por dispositivo (segundo o site oficial) | Conforme o plano e o modelo de cobrança |
| Hospedagem | Própria, nuvem pública ou Zabbix Cloud | Conforme o fornecedor (muitas são SaaS) |
| Dados | No seu banco, sob sua gestão (instalação própria) | Conforme os termos do fornecedor |
| Customização | Templates, API, webhooks, módulos e código | Conforme as opções do produto |
| Suporte | Comunidade gratuita; suporte oficial por assinatura | Conforme o plano |
| Facilidade inicial | Exige planejamento e conhecimento técnico | Frequentemente mais simples de começar |

### Quando é a melhor escolha?
- Você precisa monitorar muitos dispositivos sem custo de licença proporcional ao tamanho do parque.
- Há exigências de privacidade, LGPD ou ambientes isolados.
- Sua equipe quer adaptar o monitoramento ao próprio negócio via API e templates.

### Quando considerar com cuidado?
- Não há equipe com tempo ou conhecimento para operar, atualizar e dar backup do servidor.
- Você precisa de algo pronto em minutos, sem manutenção. Nesse caso, avalie o [Zabbix Cloud](https://www.zabbix.com/br/cloud) ou um parceiro.
- A política da empresa exige contrato e SLA de um único fornecedor (existem [assinaturas de suporte](https://www.zabbix.com/br/subscriptions) e [parceiros](https://www.zabbix.com/br/partners)).

---

## 💼 Casos de uso

| Perfil | Como ajuda |
|---|---|
| TI interna | Visão unificada de servidores, rede, links e serviços |
| MSPs | Monitoramento multicliente com proxies e permissões por grupo |
| Educação | Redes de campus e laboratórios (há o [Movimento Educacional](https://www.zabbix.com/br/educational_movement)) |
| Saúde e bancos | Controle e privacidade dos dados em ambientes regulados |
| Indústria / OT | Equipamentos industriais e IoT junto à TI |
| Home lab | Aprender monitoramento profissional sem custo de licença |

---

## 📌 Roadmap do repositório

- [ ] Guia de instalação em Debian com screenshots
- [ ] Guia de migração da 7.2 para uma versão LTS
- [ ] Biblioteca de templates e dashboards
- [ ] Exemplos de webhooks (chamados, mensageiros)
- [ ] Scripts de backup e restauração
- [ ] Playbooks Ansible de implantação de agentes
- [ ] Versão em inglês deste README

---

## 🤝 Como contribuir

1. Faça um **fork** deste repositório
2. Crie uma branch: `git checkout -b minha-melhoria`
3. Faça o commit: `git commit -m "Descreve a melhoria"`
4. Envie: `git push origin minha-melhoria`
5. Abra um **Pull Request**

Você também ajuda reportando erros, sugerindo conteúdo, traduzindo e compartilhando o projeto.

---

## ❓ FAQ

<details>
<summary><strong>O Zabbix é gratuito?</strong></summary>

O software é open source e pode ser baixado e usado sem taxa de licença. Suporte oficial, treinamentos e o Zabbix Cloud são serviços pagos opcionais.
</details>

<details>
<summary><strong>Posso usar em empresa (uso comercial)?</strong></summary>

Sim. A licença AGPLv3 permite uso comercial. Se você modificar o código e disponibilizá-lo a terceiros (inclusive por rede), deve compartilhar as modificações. Leia a [página de licença](https://www.zabbix.com/br/license).
</details>

<details>
<summary><strong>Preciso de IP fixo?</strong></summary>

Para o servidor, é recomendável um IP ou nome DNS estável. Hosts monitorados com agente ativo não precisam de IP público.
</details>

<details>
<summary><strong>Funciona atrás de NAT ou CGNAT?</strong></summary>

O agente ativo inicia a conexão de dentro para fora, o que resolve a maioria dos casos de NAT. O servidor ou proxy precisa ser alcançável pelo agente na porta configurada.
</details>

<details>
<summary><strong>Há aplicativo para celular?</strong></summary>

O site oficial anuncia o Zabbix Mobile App como “em breve”. Confira a [página do app](https://www.zabbix.com/br/mobile_app) para o status atual.
</details>

<details>
<summary><strong>Como faço backup?</strong></summary>

Faça dump regular do banco de dados (PostgreSQL ou MySQL) e copie `/etc/zabbix/`. Em Docker, inclua também o diretório `zbx_env/`. Teste a restauração periodicamente.
</details>

<details>
<summary><strong>Posso trocar a logo e as cores?</strong></summary>

Há opções de temas e módulos de frontend, mas alterações de marca devem respeitar a [política de marcas registradas](https://www.zabbix.com/br/trademark). Consulte a documentação da sua versão.
</details>

<details>
<summary><strong>Onde pedir ajuda?</strong></summary>

No [fórum oficial](https://www.zabbix.com/forum), na [documentação](https://www.zabbix.com/documentation/current/pt/manual/), no [blog](https://blog.zabbix.com/) ou com um [parceiro Zabbix](https://www.zabbix.com/br/partners).
</details>

---

## 📄 Licença e créditos

- **Licença do Zabbix:** AGPLv3 a partir da versão 7.0; versões anteriores (até a 6.4) usam GPLv2. A documentação oficial não é distribuída sob GPL. Detalhes em [zabbix.com/br/license](https://www.zabbix.com/br/license).
- **Site oficial:** <https://www.zabbix.com/br>
- **Documentação:** <https://www.zabbix.com/documentation/current/pt/manual/>
- **Código-fonte:** <https://github.com/zabbix/zabbix>
- **Imagens e Compose:** <https://github.com/zabbix/zabbix-docker>

> Este repositório é um material independente e **não é afiliado, patrocinado ou endossado** pela Zabbix LLC nem por qualquer fornecedor de soluções comerciais de monitoramento mencionado apenas para fins de comparação. Zabbix e demais marcas pertencem aos seus respectivos donos.

<div align="center">

⭐ Gostou? Deixe uma estrela no repositório e compartilhe com sua equipe.

</div>
