<div align="center">

# 🦈 FlowShark

**Automação de Verificação de Flood & Ativação de Casos de Uso**

![Version](https://img.shields.io/badge/versão-1.6-blue)
![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![Platform](https://img.shields.io/badge/plataforma-Windows-lightgrey)

</div>

---

## O que é

FlowShark é uma ferramenta de automação interna para analistas SOC da ISH. Ela automatiza o fluxo de ativação de **Casos de Uso (CDU)** no portal Vision, realizando todas as verificações necessárias — flood no ServiceNow, flood no Vision e existência de playbook no Knowledge Base — antes de efetivar a ativação.

---

## Fluxo de automação

```
Chamado (ServiceNow)
        │
        ▼
1. Coleta dados do chamado
   └─ Cliente, Company ID, número, data de abertura, CDUs detectados
        │
        ▼
2. Verifica flood no ServiceNow
   └─ Busca incidentes dos últimos 7 dias por CDU + empresa
        │
        ▼
3. Verifica flood no Vision
   └─ Filtra alertas no Offense Manager por CDU
        │
        ▼
4. Verifica playbook no Knowledge Base
   └─ Busca por similaridade no KB do ServiceNow
        │
        ▼
5. Ativa o CDU no Vision
   └─ Navega até MDR → Casos de Uso e ativa
```

---

## Instalação

### Usando o executável (recomendado)

1. Baixe a versão mais recente em **[Releases](../../releases/latest)**
2. Extraia o `.zip` e coloque os arquivos em uma pasta de sua escolha
3. Configure o `api.env` com suas credenciais (veja abaixo)
4. Execute `FlowShark.exe`

### Rodando pelo código-fonte

```bash
pip install selenium beautifulsoup4 python-dotenv customtkinter Pillow
python FlowSharkGUI.py
```

> Requer **Google Chrome** instalado. O ChromeDriver é gerenciado automaticamente pelo Selenium.

---

## Configuração (`api.env`)

Crie o arquivo `api.env` na mesma pasta do executável:

```env
# Credenciais ServiceNow (ADFS)
USUARIO_LOGIN=dominio\seu.usuario
SENHA_LOGIN=sua_senha

# Credenciais Vision
USUARIO_VISION=seu.usuario@empresa.com.br
SENHA_VISION=sua_senha

# URL de login
URL_LOGIN=https://service-now.empresa.com.br/

# Atualização automática (opcional)
URL_VERSAO=https://api.github.com/repos/alexsilva-sh/FlowShark/releases/latest
URL_DOWNLOAD=https://github.com/alexsilva-sh/FlowShark/releases/latest/download/FlowShark.exe
```

> ⚠️ **Nunca compartilhe ou suba o `api.env` em repositórios.** Ele já está no `.gitignore`.

---

## Uso

1. Abra o **FlowShark.exe**
2. Aguarde o login automático no ServiceNow
3. No menu, escolha **1 — Procedimento de ativação de Caso de Uso**
4. Cole a URL do chamado no ServiceNow (qualquer formato é aceito)
5. Confirme ou ajuste os CDUs detectados
6. Acompanhe as verificações no log e responda às interações quando solicitado

### Formatos de URL aceitos

```
https://ish.service-now.com/sn_customerservice_case.do?sys_id=abc123...
https://ish.service-now.com/nav_to.do?uri=%2Fsn_customerservice_case.do%3Fsys_id%3Dabc123...
```

Ambos os formatos são normalizados automaticamente.

---

## Compilando o executável

```bash
# 1. Gerar logo (opcional)
python gen_logo.py

# 2. Compilar com PyInstaller
pyinstaller FlowSharkGUI.spec
```

O executável gerado estará em `dist/FlowShark.exe`.

---

## Atualização automática

Ao iniciar, o FlowShark verifica silenciosamente se há uma nova versão disponível no GitHub. Se houver, um banner aparece na barra lateral com o link para download.

Para publicar uma nova versão:
1. Atualize `VERSAO_ATUAL` em `FlowSharkGUI.py`
2. Compile o executável
3. Crie uma nova Release no GitHub com a tag `vX.Y` e anexe o `FlowShark.exe`

---

## Estrutura do projeto

```
FlowShark/
├── FlowShark.py        # Lógica de automação (Selenium, scraping, fluxos)
├── FlowSharkGUI.py     # Interface gráfica (customtkinter + tkinter)
├── gen_logo.py         # Converte FlowShark.ico para base64 (build)
├── FlowShark.ico       # Ícone da aplicação
├── api.env             # Credenciais (não versionado)
└── dist/
    └── FlowShark.exe   # Executável compilado
```

---

## Requisitos

| Componente | Versão mínima |
|---|---|
| Windows | 10 / 11 |
| Google Chrome | Qualquer versão recente |
| Python (dev) | 3.10+ |

---

<div align="center">
  <sub>Uso interno — ISH Tecnologia</sub>
</div>
