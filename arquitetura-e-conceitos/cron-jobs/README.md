# ⏱️ Cron Jobs: Comandos e Conceitos Essenciais

Guia rápido de referência para o funcionamento e agendamento de **Cron jobs**, um automatizador de tarefas baseado em tempo para sistemas Unix-like (Linux/macOS).

---

## 💻 1. Comandos Essenciais

Gerencie a tabela de agendamentos do sistema operacional através do utilitário `crontab`:

* **`crontab -e`**: Edita ou cria o arquivo crontab do usuário atual.
* **`crontab -l`**: Lista todas as tarefas (cron jobs) agendadas para o usuário atual.
* **`crontab -r`**: **Cuidado!** Remove permanentemente todo o arquivo crontab do usuário atual.
* **`crontab -u usuario -e`**: Edita o crontab de um usuário específico (requer privilégios `root`/`sudo`).
* **`sudo systemctl status cron`**: Verifica se o serviço daemon do cron (`cron` ou `crond`) está ativo no sistema.

---

## 🧩 2. Sintaxe da Expressão Cron

A estrutura de tempo padrão possui **5 campos obrigatórios**, dispostos rigidamente nesta ordem:

```text
* * * * * /caminho/absoluto/do/comando.sh
│ │ │ │ │
│ │ │ │ └───── Dia da semana (0 - 7, onde 0 e 7 representam Domingo)
│ │ │ └─────── Mês (1 - 12)
│ │ └───────── Dia do mês (1 - 31)
│ └─────────── Hora (0 - 23)
└───────────── Minuto (0 - 59)
```

### Operadores Especiais
* **`*` (Asterisco)**: Significa "todos" os valores possíveis daquela coluna.
* **`,` (Vírgula)**: Separa múltiplos valores explícitos. *Ex: `0,30 * * * *` (roda nos minutos 0 e 30).*
* **`-` (Ífen)**: Define um intervalo fechado de valores. *Ex: `0 9-17 * * *` (roda de hora em hora, das 9h às 17h).*
* **`/` (Barra)**: Define passos/saltos de tempo (intervalos repetitivos). *Ex: `*/15 * * * *` (roda a cada 15 minutos).*

---

## 🚨 3. Boas Práticas e Armadilhas

* **Use Caminhos Absolutos:** O cron roda em um ambiente isolado com a variável `$PATH` extremamente reduzida. **Nunca** use caminhos relativos ou apenas o comando direto. 
  * ❌ *Incorreto:* `python3 script.py`
  *  *Correto:* `/usr/bin/python3 /home/usuario/scripts/script.py`
* **Variáveis de Ambiente:** Se o seu script depende de variáveis (`.env`), carregue-as explicitamente no script ou defina-as no topo do arquivo do crontab.
* **Redirecionamento de Saída (Logs):** Por padrão, o cron tenta enviar um e-mail local se o comando gerar qualquer saída. Para evitar lixo no sistema, direcione as saídas para um arquivo ou descarte-as:
  * Enviar para um log: `>> /home/user/cron.log 2>&1`
  * Silenciar/Descartar tudo: `> /dev/null 2>&1`
* **Validação Externa:** Na dúvida com regras complexas, utilize a ferramenta [Crontab Guru](https://crontab.guru).

---

## 📋 4. Exemplos Práticos

### Atalhos Especiais (Macros)
Alguns sistemas aceitam palavras-chave em vez de números:
* **`@reboot`**: Executa uma única vez, logo após a inicialização do sistema.
* **`@hourly`**: Equivalente a `0 * * * *` (uma vez por hora).
* **`@daily`**: Equivalente a `0 0 * * *` (uma vez por dia, à meia-noite).
* **`@weekly`**: Equivalente a `0 0 * * 0` (uma vez por semana, no domingo).
* **`@monthly`**: Equivalente a `0 0 1 * *` (uma vez por mês, no dia 1).

### Expressões de Frequência Comuns
* **`* * * * *`**: Executa a cada minuto.
* **`0 * * * *`**: Executa no início de cada hora (ex: 01:00, 02:00).
* **`0 12 * * 1-5`**: Executa às 12:00 PM, apenas de segunda a sexta-feira.
* **`*/15 9-17 * * *`**: Executa a cada 15 minutos, mas apenas dentro do bloco das 9h às 17h.
* **`0 4 * * 0`**: Executa todo domingo exatamente às 4h da manhã.

### Scripts e Comandos Reais (Sysadmin)

**Executar script Python salvando logs (Todo dia às 2h da manhã):**
```bash
0 2 * * * /usr/bin/python3 /home/usuario/scripts/backup.py >> /home/usuario/logs/backup.log 2>&1
```

**Limpar arquivos temporários antigos (Toda segunda-feira à meia-noite - remove arquivos com +7 dias):**
```bash
0 0 * * 1 /usr/bin/find /tmp -type f -mtime +7 -delete
```

**Sincronizar pastas com Rsync (A cada hora cheia):**
```bash
0 * * * * /usr/bin/rsync -avz /var/www/html/ usuario@servidor:/backup/www/
```

**Reiniciar serviço do Nginx (Todo domingo às 3h da manhã - requer crontab do root):**
```bash
0 3 * * 0 /usr/sbin/systemctl restart nginx
```
