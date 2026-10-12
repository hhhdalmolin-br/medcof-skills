---
name: "medcof-animacoes-aula"
description: "Professor MedCof envia o .pptx da aula: analisa os slides, entrega os 4 pontos que mais ganham com vídeo de IA e produz as animações no Higgsfield, com aprovação do professor em cada etapa de gasto."
---

# Animações de IA para aulas MedCof

Objetivo: tirar a aula do estático. O professor manda o .pptx. Claude devolve exatamente 4 pontos da aula que valem virar vídeo de IA, ajusta tempo e modelo com o professor e gera os vídeos usando a skill seedance-3d-cgi. Nada que gaste crédito acontece sem aprovação explícita do professor.

## Regra de ouro

Crédito só é gasto depois de um sim do professor. São três portões de aprovação:
1. Quais dos 4 pontos produzir.
2. Imagem-âncora escolhida (antes do vídeo).
3. Rascunho 480p aprovado (antes do 1080p final).

## Etapa 0. Pré-requisito: Higgsfield conectado

Antes de qualquer análise, verificar se as ferramentas mcp__Higgsfield__* existem na sessão (ToolSearch com "Higgsfield balance"). Se não existirem, parar e dizer ao professor:
"Para gerar os vídeos, o Higgsfield precisa estar conectado ao Claude. Vá em Configurações, Conectores, adicione o Higgsfield e faça login na sua conta. Depois me mande a aula de novo."
A análise dos 4 pontos pode ser entregue mesmo sem o conector, mas a geração não.

Com o conector ativo, chamar mcp__Higgsfield__balance e guardar o saldo para a estimativa de custo.

## Etapa 1. Ler a aula inteira

O .pptx é do professor. Trabalhar numa cópia.

Texto e notas:
```
python -m markitdown aula.pptx > aula.md
```

Visual (o texto sozinho não mostra as imagens, e muitas vezes a imagem é o conteúdo):
```
soffice --headless --convert-to pdf aula.pptx
pdftoppm -r 30 -png aula.pdf s
```
Montar folhas de contato de 40 slides (grade 5 colunas, número em vermelho no canto) com PIL e ler cada folha. Atenção: slides ocultos não aparecem no PDF, então a numeração renderizada pode diferir da do PowerPoint. Referenciar cada ponto pelo título do slide e pelo conteúdo, não só pelo número.

Para ver uma imagem específica em alta resolução, descompactar o .pptx e abrir ppt/media/ pelo rels do slide (ppt/slides/_rels/slideN.xml.rels).

## Etapa 2. Escolher os 4 pontos

Um trecho da aula merece vídeo de IA quando cumpre pelo menos um destes critérios:
1. Processo no tempo: progressão, padrão temporal, sequência de eventos.
2. Mecanismo escondido: a foto mostra o resultado, o vídeo mostra a causa.
3. Relação espacial em 3D: anatomia que a imagem plana não explica.
4. Alta incidência em prova: o aluno decora sem entender.

Pontuar cada candidato pelos critérios e pelo risco de erro científico do modelo de vídeo. Escolher os 4 com maior ganho. Sempre exatamente 4.

Não viram vídeo de IA, e sim animação nativa do PowerPoint (mais barata e exata): critérios de classificação, escores, tabelas, fluxogramas de tratamento, listas, take-home. Citar isso numa linha.

Formato da entrega (prosa curta por ponto, sem enrolação):
- Título do ponto
- Slides de origem (título e conteúdo)
- O que o vídeo mostra, em 2 ou 3 frases
- Por que vale (qual critério)
- Risco anatômico ou científico e como contornar

Fechar com a estimativa de custo e o saldo atual do Higgsfield.

## Etapa 3. Portão 1: aprovação e sensibilidade

Usar AskUserQuestion. Primeira pergunta, multiSelect: quais dos 4 pontos produzir.

Segunda rodada, para aprofundar a sensibilidade:
- Duração: 15 s, 30 s (recomendado para aula) ou outra.
- Formato: 16:9 para a aula ou 9:16 para Reels e redes.
- Estilo visual: 3D fotorrealista científico (recomendado), 3D estilizado didático ou ilustração médica limpa.
- Modelo de vídeo: Seedance 2.5 (recomendado, até 30 s, aceita quadro inicial e final) ou Kling 3.0 (multi-shot). Consultar mcp__Higgsfield__models_explore se o professor pedir outro.

Se o professor já tiver respondido algum item na conversa, não perguntar de novo.

Antes de gerar, rodar get_cost:true com os parâmetros escolhidos e mostrar o custo total previsto.

## Etapa 4. Gerar com a skill seedance-3d-cgi

Carregar a skill seedance-3d-cgi e seguir o fluxo dela. Em resumo:

1. Imagem-âncora inicial: gpt_image_2_5, count 4, no formato escolhido, sem texto. Baixar as 4, montar grade e escolher a anatomicamente mais correta.
2. Para vídeo de progressão de doença: gerar também a imagem do estado final, editando a âncora (medias com role image_references apontando para a âncora inicial, mesmo enquadramento). Escolher a melhor.
3. Portão 2: mostrar ao professor as âncoras escolhidas (SendUserFile) e pedir aprovação antes do vídeo.
4. Vídeo: seedance_2_5, mode omni_reference, start_image e, quando houver, end_image; generate_audio false; draft true (480p). Timeline com segundos exatos por beat, câmera com easing, final parado 1,5 s para o professor rotular.
5. Revisar o rascunho antes de mostrar: baixar o mp4, extrair quadros em 1, 5, 8, 11, 14, 17, 20, 23, 26 e 29,5 s com ffmpeg, montar grade e conferir anatomia, sequência e se o vídeo respeitou o quadro inicial e o final.
6. Portão 3: enviar o rascunho com uma avaliação honesta do que ficou bom e do que ficou errado, com o segundo exato de cada defeito. Perguntar se finaliza em 1080p ou refaz.
7. Final: generate_video com draft_job_id do rascunho aprovado e resolution 1080p. Entregar o mp4 final e perguntar se o professor concorda com o resultado antes de qualquer nova geração.

## Custos de referência (Seedance 2.5, out/2026)

- Imagem gpt_image_2_5: 0,25 crédito cada.
- Rascunho 480p de 30 s: 90 créditos.
- Finalizar rascunho em 1080p: 60 créditos.
- 1080p direto de 30 s: 360 créditos. Nunca fazer direto.
- Gerações barradas pelo filtro não são cobradas.
Sempre confirmar com get_cost, porque os preços mudam.

## Lições já aprendidas

- O modelo às vezes ignora o start_image e inventa outra cena. Travar o end_image resolve e também segura a anatomia final.
- Patologia descrita em linguagem crua (tecido vermelho carnoso, invasão, carving, punched-out) dispara o filtro de conteúdo. Reescrever em termos neutros e didáticos: joint lining, thickened tissue, small notches, educational textbook visualization.
- Destacar só a placa subcondral é difícil: o modelo acende o osso trabecular inteiro. Descrever espessura em milímetros e dizer o que não deve acender.
- Destaque fraco quando a cor do destaque é próxima da cor original (cápsula branca com brilho prateado). Usar cor de destaque contrastante.
- Erro 429 na geração de imagem é limite de taxa. Esperar alguns segundos e repetir a mesma chamada.
- Nunca pedir texto, rótulo ou seta no vídeo. Rótulos entram depois no PowerPoint.
- Pedir patologia e anatomia normal no mesmo clipe confunde o modelo. Separar.

## Estilo da comunicação com o professor

Frases curtas e diretas. Dado antes de opinião. Dizer claramente o que ficou errado no rascunho. Sem promessa de qualidade antes de revisar os quadros.