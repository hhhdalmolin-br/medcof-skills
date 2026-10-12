# MedCof Skills: animações de IA para aulas

Duas skills do Claude que transformam slides estáticos em vídeos 3D gerados por IA no Higgsfield.

O professor envia o .pptx da aula. O Claude lê todos os slides e devolve os 4 pontos que mais ganham com vídeo. Depois ajusta duração, formato, estilo e modelo com o professor e gera as animações. Nenhum crédito é gasto sem aprovação.

## As duas skills

| Skill | Função |
| --- | --- |
| `medcof-animacoes-aula` | Principal. Analisa o .pptx, escolhe os 4 pontos, conduz as aprovações e entrega o vídeo final. |
| `seedance-3d-cgi` | Motor de geração. Escreve os prompts de vídeo 3D para o Seedance no Higgsfield com regras de anatomia. |

A principal chama a `seedance-3d-cgi` na etapa de geração. Instale as duas.

## Instalação

1. Baixe os dois arquivos da pasta [`dist/`](dist/): `medcof-animacoes-aula.zip` e `seedance-3d-cgi.zip`.
2. No Claude, abra as configurações, vá até a área de Skills e envie cada .zip.
3. Nos conectores do Claude, conecte o **Higgsfield** e faça login com a sua conta. Os créditos usados serão os da sua conta.

## Como usar

Abra uma conversa nova, anexe o .pptx da aula e escreva:

> Analisa essa aula para animações de IA.

Ou chame direto: `/medcof-animacoes-aula`.

## O fluxo

1. **Análise.** O Claude lê o texto e a imagem de cada slide e entrega 4 pontos. Cada ponto traz os slides de origem, o que o vídeo mostra, o motivo e o risco científico.
2. **Aprovação 1.** Você escolhe quais pontos produzir e define duração, formato (16:9 aula ou 9:16 Reels), estilo e modelo. O Claude mostra o custo previsto.
3. **Aprovação 2.** O Claude gera 4 opções de imagem-âncora e mostra a escolhida antes de fazer o vídeo.
4. **Aprovação 3.** Sai um rascunho em 480p com a avaliação do que ficou bom e do que ficou errado, segundo a segundo. Você decide se finaliza ou refaz.
5. **Final.** Vídeo em 1080p, sem texto. Os rótulos entram depois no PowerPoint.

## Critérios de escolha dos 4 pontos

Um trecho da aula vira vídeo quando tem pelo menos um destes:

1. Processo no tempo
2. Mecanismo escondido
3. Relação espacial em 3D
4. Alta incidência em prova

Critérios de classificação, escores, tabelas e fluxogramas ficam com a animação nativa do PowerPoint, que é mais barata e exata.

## Custos de referência (Seedance 2.5, out/2026)

| Item | Créditos |
| --- | --- |
| Imagem-âncora | 0,25 cada |
| Rascunho 480p de 30 s | 90 |
| Finalizar o rascunho em 1080p | 60 |
| Gerações barradas pelo filtro | 0 |

Um clipe de 30 s aprovado custa cerca de 150 créditos. Os preços mudam. O Claude confere o valor antes de cada geração.

## Atualizar

As skills evoluem com o uso. Quando sair versão nova aqui, baixe o .zip de novo e substitua a skill no Claude.

Mantido por Dr. Henrique Dalmolin.
