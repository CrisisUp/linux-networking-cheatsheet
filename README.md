# 🐧 Guia de Consulta Rápida: Linux e Redes (CLI)

Um guia prático de referência rápida contendo comandos essenciais de terminal, inspeção de redes, gerenciamento de sistemas, regras de firewall e ferramentas de diagnóstico para ambientes Linux.

---

## 📚 Índice

- [Redes e Conectividade](#redes-e-conectividade)
- [Inspeção de Portas e Tráfego](#inspeção-de-portas-e-tráfego)
- [Gerenciamento de Processos e Serviços](#gerenciamento-de-processos-e-serviços)
- [Firewall e NAT (iptables)](#firewall-e-nat-iptables)
- [Monitoramento de Recursos (CPU, Memória e Disco)](#monitoramento-de-recursos-cpu-memória-e-disco)
- [Guia de Significados e Parâmetros](#-guia-de-significados-e-parâmetros)
- [Autor e Licença](#-autor-e-licença)

---

## Redes e Conectividade

```bash
# Exibir endereços IP e interfaces ativas (substituto moderno do ifconfig)
ip a

# Exibir tabela de roteamento IP
ip route show

# Testar conectividade e rota ICMP com contagem de pacotes
ping -c 4 8.8.8.8

# Rastrear caminho de pacotes (saltos) até o destino
traceroute 1.1.1.1
```

## Inspeção de Portas e Tráfego

```bash
# Listar sockets TCP/UDP abertos e portas em escuta (substituto do netstat)
ss -tulpn

# Verificar se uma porta específica está acessível (ex: porta 80)
nc -zv 192.168.1.1 80

# Capturar pacotes em tempo real em uma interface de rede específica
sudo tcpdump -i eth0 port 443
```

## Gerenciamento de Processos e Serviços

```bash
# Verificar status, iniciar ou reiniciar um serviço systemd
sudo systemctl status nginx
sudo systemctl restart networking

# Monitorar processos e consumo de recursos em tempo real
htop

# Encontrar o PID de um processo pelo nome exato
pgrep -la sshd
```

## Firewall e NAT (iptables)

```bash
# Listar todas as regras atuais com contadores de pacotes e números de linhas
sudo iptables -L -v -n --line-numbers

# Listar especificamente as regras da tabela NAT (Port Forwarding e Masquerading)
sudo iptables -t nat -L -v -n --line-numbers

# Permitir tráfego na interface de loopback (localhost)
sudo iptables -A INPUT -i lo -j ACCEPT

# Permitir conexões já estabelecidas e relacionadas (essencial para não derrubar sessões)
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Habilitar o Masquerading (NAT de saída para compartilhar internet pela interface eth0)
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Redirecionar uma porta externa para um IP/porta interna (Port Forwarding / DNAT)
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.50:80

# Salvar as regras atuais para persistirem após o reboot (em sistemas baseados em Debian/Ubuntu)
sudo iptables-save > /etc/iptables/rules.v4
```

## Monitoramento de Recursos (CPU, Memória e Disco)

```bash
# Monitorar processos, CPU e uso de memória em tempo real (versão moderna do top)
htop

# Verificar o espaço em disco disponível e partições montadas (modo human-readable)
df -h

# Medir o uso de espaço de diretórios específicos (ex: logs ou home)
du -sh /var/log/* | sort -hr

# Exibir uso de memória RAM e Swap detalhado em megabytes
free -m

# Inspecionar uso de CPU por núcleo e estatísticas globais de I/O
mpstat -P ALL 1

# Monitorar E/S de disco e requisições de leitura/escrita em tempo real
iostat -xz 2
```

## 📖 Guia de Significados e Parâmetros

### Rede e Diagnóstico

- `ip a` — Lista todas as interfaces de rede e seus respectivos endereços IP configurados.
- `ip route` — Exibe a tabela de rotas do núcleo do sistema, indicando o gateway padrão.
- `ss` — Ferramenta rápida para despejar estatísticas de sockets (substituta mais eficiente que o antigo netstat).
- `tcpdump` — Analisador de pacotes de linha de comando para interceptar e exibir tráfego de rede.

### Parâmetros do iptables

- `-A` — Adiciona (Append) uma regra ao final da cadeia (Chain).
- `-i / -o` — Especifica a interface de entrada (Input) ou saída (Output).
- `-j` — Define a ação (Jump) a ser tomada (ex: ACCEPT, DROP, MASQUERADE, DNAT).
- `-t nat` — Seleciona a tabela NAT, usada para tradução de endereços de rede.
- `--dport` — Porta de destino (Destination Port).
- `-m conntrack` — Módulo de rastreamento de estado de conexão.

### Monitoramento de Recursos

- `df -h` — O parâmetro -h (human-readable) converte os blocos de disco para unidades legíveis (MB, GB, TB).
- `free -m` — Exibe a memória em Megabytes, facilitando a leitura rápida do consumo livre e em cache.
- `du -sh` — Resume (s) o tamanho total de arquivos/pastas em formato amigável (h).

- `htop` — Visualizador interativo de processos que usa cores e barras visuais para facilitar a leitura de carga da CPU e RAM.

## 👤 Autor e Licença

Autor: Cristiano Batista Pessoa

Licença: Este projeto está licenciado sob a Licença MIT, permitindo uso livre, modificação e distribuição.
