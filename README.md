# dr_cassio — prompts da assistente virtual (Clínica Fenice)

Prompts da automação de atendimento do **Dr. Cássio Henrique Vazão** (Clínica Fenice, Olímpia/SP),
consumidos em tempo de execução pelos fluxos n8n via `raw.githubusercontent.com`.

| Arquivo | Consumido por | Node |
|---|---|---|
| `prompt_secretaria.md` | ATENDIMENTO `1g8a9O8wH8F0xnSA` | `GET GitHub` |
| `prompt_confirmacao.md` | ATENDIMENTO `1g8a9O8wH8F0xnSA` | `GET GitHub1` |
| `prompt_dr.md` | OUVIDO `7N013y77Zxig6xuR` | `GET GitHub` |

> ⚠️ **Não renomeie nem apague estes arquivos.** O fluxo os busca pelo nome exato.
> Um arquivo ausente faz o agente subir **sem prompt**, em produção, silenciosamente.
> (Aconteceu no Gavira em 20/07/2026.)

Arquitetura da agenda: **Google Calendar é a fonte de verdade**
(`clinicafenice.olp@gmail.com`). Doctoralia não integra — a API é fechada a
parceiros PMS.

---

## Estado: PROVISÓRIO

Estes prompts entraram no ar com **travas de segurança** onde a clínica ainda
não confirmou a informação. Cada trava faz a assistente **escalar para humano**
em vez de arriscar um erro. Ao receber a resposta, remova a trava.

| Trava ativa hoje | Libera quando | Onde mexer |
|---|---|---|
| Não informa **chave Pix** | confirmarem a chave (há divergência: `99677-0762` vs `99766-0762`) | `prompt_secretaria.md` › "Assuntos que você NÃO resolve" + "Dados da clínica" |
| Não afirma nada sobre **convênios** | disserem se é 100% particular ou quais aceitam | idem |
| Não agenda **telemedicina** | definirem como o link chega ao paciente (fluxo novo, não existe no clone) | idem + fluxo n8n a construir |
| Não informa preço de **soroterapia, cannabis, check up, idoso, obesidade, diabetes** | confirmarem valor e duração | idem |
| Não agenda **domiciliar** direto | resolverem a duplicidade de cadastro (40min vs 20min, mesmo preço) | idem |
| **Recusa psiquiatria** | — permanente. Dr. Cássio é clínico geral; a lista do Doctoralia veio poluída com o template padrão da plataforma | idem |

## Valores assumidos (podem mudar)

| Item | Valor no prompt | Origem |
|---|---|---|
| Grade | 20 min | assumido — a clínica cita 20/30/40 min |
| Almoço | 12h-13h | **assumido, não confirmado** |
| Segunda-feira | fechada | assumido — "segunda fechada plantão" é ambíguo |
| Secretária | tratada como "a recepção" | nome ainda não definido |

⚠️ A grade e o almoço vivem em **dois lugares** e precisam bater:
1. este prompt (seção "Dados da clínica")
2. o bloco `CFG` no node `Prep` do fluxo DISPONIBILIDADE `F3cGM7BAS6S7zWEu`

## Confirmado pela clínica

```
Dr. Cássio Henrique Vazão · CRM/SP 224885 · clínico geral (criança ao idoso)
Av. Amélia Seno Maziteli (Av. Ferrasa), 76 — Di Vitória Condominium
Olímpia/SP · CEP 15405-256 · clinicafeniceolimpia@gmail.com

Ambulatorial R$ 300  ·  Domiciliar R$ 350  ·  Telemedicina R$ 250
ECG R$ 120           ·  Risco cirúrgico R$ 80

Atestado e pedido de exame ... só com consulta
Receita ..................... por e-mail ou WhatsApp
Medicação nova .............. só com exames + consulta
Troca de medicação .......... decisão do médico (o prompt escala)
Retorno ..................... 1 incluso em até 30 dias
Pagamento ................... dinheiro, débito, crédito, Pix
```

## Ao editar

Depois de qualquer alteração aqui, **commit + push**. Os fluxos leem a branch
`main` com `?nocache=`, então a mudança vale na execução seguinte — sem
necessidade de republicar o workflow.
