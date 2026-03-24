# Meeting Manager — Microsoft Teams App

App para gerenciamento de pautas e controle de tempo em reuniões do Microsoft Teams.  
Desenvolvido por **SysManager Informática LTDA** — *Integrando Mundos*.

---

## Funcionalidades

- **Pautas cronometradas** — defina título, tempo previsto e responsável para cada pauta
- **Cronômetro global** — tempo total da reunião sempre visível
- **Alerta de estouro** — barra de progresso muda para vermelho ao ultrapassar o tempo previsto
- **Controle de fala por participante** — clique no card do participante para ativar/pausar o cronômetro de fala
- **Auto-detecção do usuário Teams** — o participante logado é adicionado automaticamente
- **Resumo exportável** — ao encerrar, gera tabela de pautas (status, tempo realizado) e ranking de fala por participante em `.txt`
- **Suporte a dark mode** — adapta automaticamente ao tema do Teams
- **Funciona em**: aba de canal, chat em grupo, painel lateral de reunião e palco da reunião

---

## Estrutura do repositório

```
meeting-manager/
├── index.html           # App principal (interface completa)
├── config.html          # Tela de configuração da aba no Teams
├── manifest.json        # Manifesto oficial do app Teams
├── icons/
│   ├── icon-color.png   # Ícone colorido 192×192px (Teams Admin Center)
│   └── icon-outline.png # Ícone outline 32×32px (barra lateral do Teams)
└── README.md            # Este arquivo
```

---

## Pré-requisitos

- Conta **GitHub** (para hospedar via GitHub Pages)
- Acesso ao **Microsoft Teams Admin Center** (`admin.teams.microsoft.com`)  
  ou permissão de **sideload** no Teams
- Tenant **Microsoft 365** (qualquer plano)

---

## Instalação via Git + GitHub Pages

### 1. Clonar o repositório

```bash
git clone https://github.com/kyew22/meeting-manager.git
cd meeting-manager
```

### 2. Ativar o GitHub Pages

Acesse `https://github.com/kyew22/meeting-manager/settings/pages` e configure:

- **Source**: Deploy from a branch
- **Branch**: `main` → `/ (root)`
- Clique em **Save**

Após 1-2 minutos, o app estará disponível em:

```
https://kyew22.github.io/meeting-manager/index.html
```

### 3. Verificar o deploy

Abra a URL acima no navegador. O Meeting Manager deve carregar normalmente **sem pedir login**.  
Esse é o requisito para funcionar dentro do Teams (iframe sem autenticação).

---

## Publicação no Microsoft Teams

### Opção A — Teams Admin Center (toda a organização)

1. Acesse `https://admin.teams.microsoft.com`
2. No menu lateral: **Aplicativos do Teams** → **Gerenciar aplicativos**
3. Clique em **Carregar** → selecione o arquivo `meeting-manager-teams.zip`
4. Após o upload, clique no app na lista → **Instalar para todos**

> **Atenção:** apps recém-carregados aparecem como "Bloqueado" por padrão.  
> É necessário clicar em **Instalar para todos** para liberar para a organização.

### Opção B — Sideload (uso pessoal, sem precisar de admin)

1. Abra o **Microsoft Teams**
2. Clique em **Apps** na barra lateral esquerda
3. Clique em **Gerenciar seus aplicativos**
4. Clique em **Carregar um aplicativo** → **Carregar um aplicativo personalizado**
5. Selecione o `meeting-manager-teams.zip`
6. Clique em **Adicionar**

---

## Como gerar o pacote .zip para o Teams

O arquivo de upload deve conter **exatamente 3 arquivos na raiz** (sem subpastas):

```bash
# macOS / Linux
zip -j meeting-manager-teams.zip manifest.json icons/icon-color.png icons/icon-outline.png

# Verificar conteúdo
unzip -l meeting-manager-teams.zip
```

No **Windows**: selecione os 3 arquivos no Explorer → botão direito → **Compactar em arquivo ZIP**.

---

## Como usar o app em uma reunião

1. Dentro de uma reunião no Teams, clique no **+** na barra superior
2. Pesquise **Meeting Manager**
3. Clique em **Adicionar** → **Salvar**

**Aba Configuração**
- Defina nome e horário da reunião
- Cadastre participantes (nome + cargo/área)
- Adicione pautas com título, tempo previsto e responsável

**Aba Reunião**
- Clique em **▶ Iniciar** para começar os cronômetros
- Clique em **Próxima pauta →** para avançar
- Clique no card de um participante para cronometrar sua fala
- Use **Pular** para pular uma pauta sem marcar como concluída
- Use **⏸ Pausar** para intervalos

**Aba Resumo**
- Exibe tempo realizado vs. previsto por pauta
- Mostra tempo de fala e % por participante
- Botão **Exportar .txt** gera o resumo para registro

---

## Atualizar o app (nova versão)

1. Edite os arquivos desejados
2. **Incremente a versão** no `manifest.json`:
   ```json
   "version": "1.0.2"
   ```
3. Commit e push:
   ```bash
   git add .
   git commit -m "chore: bump version to 1.0.2"
   git push origin main
   ```
4. Gere novo `.zip` e faça upload no Teams Admin Center  
   usando **"Fazer upload de arquivo"** na página do app existente

> O Teams rejeita uploads com o mesmo número de versão — sempre incremente antes de reenviar.

---

## Personalizar para outro tenant

Se for fazer fork ou adaptar para outra organização, ajuste no `manifest.json`:

| Campo | Valor atual | Descrição |
|---|---|---|
| `id` | `057feac2-4ae4-4ad3-aaba-bd8d83217c15` | Gere um novo GUID em [guidgenerator.com](https://guidgenerator.com) |
| `contentUrl` | `https://kyew22.github.io/...` | URL do `index.html` hospedado |
| `configurationUrl` | `https://kyew22.github.io/...` | URL do `config.html` hospedado |
| `validDomains` | `kyew22.github.io` | Domínio do servidor de hospedagem |
| `webApplicationInfo.resource` | `https://kyew22.github.io/...` | URL base do app |

---

## Requisitos técnicos

- O servidor de hospedagem **deve servir HTTPS** (GitHub Pages já inclui)
- O `index.html` **não pode exigir autenticação** para ser carregado (o Teams usa iframe)
- O `manifest.json` deve ter `manifestVersion: "1.17"` ou superior para suporte a `meetingStage`
- Permissões RSC obrigatórias: `MeetingStage.Write.Chat` e `OnlineMeetingParticipant.Read.Chat`

---

## Histórico de versões

| Versão | Data | Alteração |
|---|---|---|
| 1.0.1 | Mar 2026 | Publicação no Teams Admin Center — GitHub Pages como host |
| 1.0.0 | Mar 2026 | Criação — interface completa, cronômetros, resumo exportável |

---

*SysManager Informática LTDA — Integrando Mundos*
