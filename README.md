# 🕒 Como Agendar a Execução de um Script Python Diariamente às 12h (Windows e Linux)

Este guia mostra **três métodos** para agendar a execução automática de um script Python todos os dias ao meio-dia:

1. Usando o **Agendador de Tarefas do Windows (interface gráfica)**
2. Usando **PowerShell**
3. Usando o **Crontab no Linux**

---

## ✅ 1. AGENDADOR DE TAREFAS (INTERFACE GRÁFICA - WINDOWS)

### Passo 1: Abrir o Agendador de Tarefas
- Pressione `Win + S` e digite **Agendador de Tarefas**
- Clique em **Criar Tarefa...**

### Passo 2: Aba "Geral"
- Nome: `Rodar Script Python`
- Marque: **Executar com os privilégios mais altos**
- Marque: **Executar se o usuário estiver conectado ou não**

### Passo 3: Aba "Disparadores"
- Clique em **Novo...**
- Configurar:
  - **Iniciar a tarefa**: Diariamente
  - **Repetir a cada**: 1 dia
  - **Hora**: 12:00:00

### Passo 4: Aba "Ações"
- Clique em **Novo...**
- Ação: **Iniciar um programa**
- Programa/script:
  ```
  C:\Users\SeuUsuario\AppData\Local\Programs\Python\Python310\python.exe
  ```
- Adicionar argumentos:
  ```
  "C:\caminho\para\seu_script.py"
  ```

### Passo 5: Aba "Condições" e "Configurações"
- Desmarcar: **Somente com energia elétrica**
- Marcar: **Executar a tarefa o quanto antes, se o agendamento for perdido**

---

## ⚡️ 2. USANDO POWERSHELL (WINDOWS)

Execute o seguinte comando no PowerShell **como administrador**, ajustando os caminhos:

```powershell
$Action = New-ScheduledTaskAction -Execute "C:\Python310\python.exe" -Argument "C:\scripts\meu_script.py"
$Trigger = New-ScheduledTaskTrigger -Daily -At 12:00PM
$Principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest
Register-ScheduledTask -TaskName "RodarScriptPythonDiariamente" -Action $Action -Trigger $Trigger -Principal $Principal
```

### Testar a tarefa manualmente:
```powershell
Start-ScheduledTask -TaskName "RodarScriptPythonDiariamente"
```

### Remover a tarefa:
```powershell
Unregister-ScheduledTask -TaskName "RodarScriptPythonDiariamente" -Confirm:$false
```

### Usar com seu usuário (opcional):
```powershell
$Principal = New-ScheduledTaskPrincipal -UserId "$env:USERNAME" -LogonType Interactive -RunLevel Highest
```

---

## 🐧 3. USANDO CRONTAB (LINUX/UNIX/MAC)

### Passo 1: Abrir o crontab
```bash
crontab -e
```

### Passo 2: Adicionar a linha:
```
0 12 * * * /usr/bin/python3 /caminho/para/seu_script.py
```

> Essa linha significa: execute o script todos os dias (`* *`) ao meio-dia (`0 12`)

### Verificar as tarefas agendadas:
```bash
crontab -l
```

### Remover o agendamento:
```bash
crontab -e
# E apague a linha correspondente
```

---

## ✅ DICA EXTRA: USAR ARQUIVO `.BAT` (WINDOWS)

Crie um arquivo chamado `rodar_script.bat` com o conteúdo:

```bat
@echo off
C:\Python310\python.exe "C:\scripts\meu_script.py"
```

E agende esse `.bat` no Agendador de Tarefas se preferir.

---

## 📌 Resumo

| Método         | Sistema  | Horário | Automático | Interface |
|----------------|----------|---------|------------|-----------|
| Agendador GUI  | Windows  | 12:00   | Sim        | Sim       |
| PowerShell     | Windows  | 12:00   | Sim        | Não       |
| Crontab        | Linux/Mac| 12:00   | Sim        | Não       |

---

📤 **Agora é só escolher a forma mais conveniente e deixar o script Python rodando sozinho todo dia às 12h!**
