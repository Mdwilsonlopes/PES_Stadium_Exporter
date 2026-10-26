# Arena Gramadense: escuridão resolvida pela restauração da camada efetiva de luz

Data: 26/10/2026. Ambiente do caso: PES 2021/Football Life, instalação TransferPatch. Estado: **sucesso em teste no jogo confirmado pelo usuário**, após a intervenção descrita abaixo. O agente verificou os arquivos; a observação visual foi realizada pelo usuário. Não há captura final arquivada nem confirmação individual de todas as condições de clima/câmera.

## Descoberta que deve orientar novos diagnósticos

Uma pasta de estádio pode conter fontes FOX válidas e bem orientadas e ainda ficar escura quando não contém a camada de iluminação que a versão funcional carregava por addon. Compare a cadeia efetiva **Stadium Server → seleção de addon → light FPK/FPKD → atmosfera/probes → LUTs**. Auditar apenas a pasta principal ou acertar o ID não garante equivalência com a referência funcional.

Por solicitação do usuário, esse padrão passa a ser uma **prioridade inicial de investigação** em estádios modificados com campo escuro/jogadores em silhueta, inclusive de dia. É uma solução confirmada neste caso, não uma medição estatística da causa mais frequente nem autorização para copiar Jbpes para todos os estádios.

## Sequência de testes e evidências

1. A referência funcional do mesmo estádio continha AddOn/Jbpes/Global; a versão modificada não continha essa camada. O modelo principal modificado era diferente do original e precisava ser preservado.
2. Havia inconsistências 058/009/059 nos arquivos, mapas e seleção. Foi preparado um candidato 059 e depois corrigidas apenas as duas entradas da Arena nos mapas. A conversão, isoladamente, não resolveu.
3. O primeiro teste negativo ainda selecionava 058 contra arquivos 059. Após corrigir os mapas, um novo log confirmou **059** e `timeofday=0, weather=0, season=0`; o usuário continuou vendo escuridão. Portanto não era suficiente atribuir o defeito ao ID ou tratá-lo exclusivamente como problema noturno.
4. A iluminação principal tinha 112 fontes customizadas registradas e orientadas conforme os alvos preservados do projeto; ela era diferente da configuração Jbpes efetiva da referência. Fontes corretas em um arquivo não provam que aquele arquivo reproduz a iluminação funcional.
5. Com autorização, foram copiados **os quatro pacotes originais de luz do Jbpes da própria Arena funcional**, mais suas **16 texturas LUT**, para os caminhos equivalentes da Arena modificada. A aplicação direta nos caminhos principais tornou o teste independente de ativar um novo addon. Não foi necessário restaurar o addon inteiro.
6. Após reiniciar e testar, o usuário confirmou: **“teste realizado com sucesso”**. A combinação restaurada constitui a solução funcional relatada neste caso. Não foi isolada a contribuição individual de cada pacote ou LUT.

## Intervenção exata

Origem relativa na referência:

`AddOn/Jbpes/Global/`

Destino relativo na Arena modificada, mantendo ID 059:

```text
Asset/model/bg/st059/light/#Win/light_st059_af.fpk
Asset/model/bg/st059/light/#Win/light_st059_af.fpkd
Asset/model/bg/st059/light/#Win/light_st059_ar.fpk
Asset/model/bg/st059/light/#Win/light_st059_ar.fpkd
Asset/model/bg/common/lut/#windx11/lut_{h,s}_{cloudy,day,evening,night}_{demo,game}.ftex
```

O padrão LUT acima representa os 16 arquivos existentes da referência, não arquivos a inventar. Consulte os nomes e SHA-256 no [manifesto deste caso](casos/arena-gramadense-jbpes-2026-10-26.json). Não se deve inferir o clima efetivo apenas dos sufixos AF/AR.

Foram substituídos quatro arquivos, acrescentados 16 e preservados byte a byte os outros **96 arquivos** da versão modificada. Modelo principal, pitch, torcida, efeitos e mapas não foram alterados nessa intervenção. Não houve aumento manual de potência, nova rotação de fontes, ajuste arbitrário de exposição nem transplante de outro estádio. Os arquivos conhecidos como funcionais foram usados sem recompilação na aplicação.

Os dois FPK contêm os recursos internos de atmosfera/probes st059 e o sky_dome referidos pelos FOX. As LUTs foram verificadas nos caminhos correspondentes. `sample_sky.ftex` permanece como dependência do motor, igual à referência; seu conteúdo no jogo-base não foi inspecionado. Fontes com pais foram preservadas, sem alegar nova validação mundial de todas elas.

## Verificação e regressão

- Comparação SHA-256 com Doctor Stadium: 4 modificados, 16 acrescentados, 96 intactos, nenhum removido.
- Os 20 arquivos instalados coincidem com a camada Jbpes da referência do mesmo estádio.
- Reextração dos quatro pacotes instalados: dez membros comparados byte a byte com a referência e dois FOX relidos pelo conversor.
- Verificação determinística anterior detectou quatro pacotes diferentes e 16 LUTs ausentes; posterior confirmou a restauração e a preservação dos demais arquivos.
- A verificação automática protege a equivalência da camada e os arquivos preservados. Ela não executa o renderizador nem reproduz sozinha a escuridão. A evidência funcional é o teste humano posterior, confirmado pelo usuário.

## Procedimento reutilizável

1. Preserve uma cópia e o manifesto da versão aprovada antes de novas modificações.
2. Confirme pasta/ID realmente carregados em log novo; examine mapas, seleção manual e sobreposições. Reinicie a partida após mudar recursos.
3. Compare a referência funcional completa, incluindo addon_config.ini, Global, variantes por clima, fontes FOX, atmosfera, probes e LUTs. Separe luz projetada de lentes/flares.
4. Se a referência do **mesmo estádio** usa uma camada ausente, prepare a recuperação mínima dessa camada e suas dependências. Preserve modelo e texturas fora do escopo; não copie toda a pasta de outro estádio.
5. Verifique hashes, membros dos pacotes e referências. Confira eixo FOX -Y e hierarquias quando a hipótese for orientação; não aplique correção universal.
6. Aplique apenas dentro da autorização existente, com backup e reversão. Este registro não concede autorização automática de instalação.
7. Repita a condição que falhava e registre resultado no jogo. Depois amplie para dia/noite, seco/chuva, abertura, câmera principal e replay. Não marque condições não observadas como aprovadas.

## Uso neste repositório

Este documento registra conhecimento do fluxo de estádios. Não implementa reparador automático, parser de Sider/FOX, recurso de interface ou funcionalidade nova no aplicativo hospedeiro. No Doctor Stadium, comparação de hashes continua exclusivamente em leitura; a aplicação deste caso foi uma operação externa autorizada. Não altera o bloqueio da pesquisa REA A42.

## Preservação e publicação

O manifesto associado contém somente caminhos relativos, tamanhos e hashes da camada. Não são publicados modelos, texturas, executáveis, logs integrais, credenciais ou caminhos pessoais. As cópias e backups dos arquivos permanecem no ambiente do usuário.