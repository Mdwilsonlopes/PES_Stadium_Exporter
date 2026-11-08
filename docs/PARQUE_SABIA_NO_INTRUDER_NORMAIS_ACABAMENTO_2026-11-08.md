# Parque do Sabiá: faixas herdadas, normais FOX e acabamento entre setores

Registro de 08/11/2026 para Blender 4.5.14 LTS, PES 2021 e Football Life 26.

## Estado comprovado

- **Sabiá ID 018, v38: aprovado pelo usuário em partida.** As faixas invasoras foram eliminadas e as partes escuras da pista e das arquibancadas desapareceram.
- **Sabiá v39: arquivos preparados, exportados, inspecionados e instalados.** Prévia real do Blender revisada; novo teste em partida pendente.
- **Kirov Stadium 85, ID 019: proteção No Intruder instalada e auditada.** Teste no jogo pendente. Não confundir com Kirov Stadium 90 nem com resíduos de outras bases.

Este documento registra conhecimento e procedimentos executados na instalação local. Publicá-lo não incorpora automaticamente funções aos programas deste repositório, não modifica seus limites atuais e não certifica outras instalações.

## 1. Um MAIN limpo não impede faixas de outro estádio

Sintoma: o Blender e as malhas autorais não continham as faixas superiores com **Tesoro del Mundo**, **La Grande**, **Calcio** e **Tradición Restablecida**, mas elas apareciam em partida.

O log confirmou a versão e o ID selecionados. A inspeção rastreou a rota separada:

```text
Asset/model/bg/common/ad/#Win/ad_stXXX_<familia>.fpk
Asset/model/bg/common/ad/#Win/ad_stXXX_<familia>.fpkd
```

O candidato anterior não fornecia esses recursos comuns. O carregador podia retornar aos pacotes nativos, apesar de a arquitetura principal ser inteiramente autoral. O atlas `ad_common_bsm` foi extraído e inspecionado: continha exatamente as quatro artes. A malha correspondente ficava aproximadamente entre 11,7 e 20 m de altura, comprovando que eram as faixas superiores, não os LEDs junto ao campo.

### Correção e prevenção No Intruder

Foram gerados pares próprios para sete famílias: `normal`, `acl`, `cl`, `el`, `olc`, `sc` e `eu`. Neste caso, cada FPK ficou sem modelos e cada FPKD conservou somente um FOX DataSet vazio no caminho virtual esperado. **FPK vazio isolado não basta:** o pacote de referências também precisa ser auditado para não continuar apontando ao visual herdado.

O helper local `NoIntruderGuard.py` passou a ser chamado antes da exportação MAIN no PES Stadium Exporter instalado. Sua política:

1. Auditar todos os pares existentes antes de escrever.
2. Bloquear pacote herdado, par incompleto ou visual desconhecido.
3. Gerar pares próprios somente quando não houver recurso autoral existente.
4. Aceitar um visual autoral apenas com origem, autorização e hashes registrados; rejeitar adulteração e referências ao atlas proibido.
5. Registrar os pares em `NoIntruder.json` e repetir a auditoria após a compilação e antes da instalação.

O BannerServer local foi ajustado para dar prioridade aos recursos comuns do estádio **somente quando existir o marcador NoIntruder.json**. Os demais estádios mantiveram o comportamento anterior. Foram preservadas a configuração e a rota dos LEDs de publicidade no campo.

Não apagar uma textura global compartilhada nem remover toda a publicidade de competição. A correção pertence ao recurso e à precedência responsáveis pela invasão. Em outra instalação ou versão do exportador, verificar a presença efetiva do hook e do módulo; uma regra escrita na documentação não instala um bloqueio.

### Regressões executadas

Oito testes do guard passaram: pacote nativo real rejeitado; par próprio aceito; FPK vazio com FOX herdado rejeitado; visual desconhecido bloqueado; par ausente retém a liberação; visual autoral com origem e hashes aceito; alteração no autorizado rejeitada; configuração LED preservada.

O operador real do Blender cancelou uma exportação contaminada antes de escrever MAIN e concluiu uma exportação limpa. Quatro testes em Lua 5.1 verificaram prioridade dos pares protegidos em competição, comportamento anterior em estádio desprotegido, rota LED preservada e reescrita UEFA resolvendo o par protegido.

O usuário confirmou a ausência das faixas na v38 em partida. A aplicação ao Kirov ID019 repetiu a geração e a auditoria dos sete pares, sem alterar modelos, iluminação ou LEDs anteriores; sua confirmação visual permanece pendente.

## 2. Superfícies pretas não significam necessariamente pouca luz

No teste problemático, campo e jogadores estavam iluminados, enquanto pista e partes das arquibancadas ficavam pretas. A investigação separou carregamento, iluminação efetiva, materiais e orientação das superfícies.

### Mapa neutro em formato errado

O mapa autoral tinha sido codificado como normal RGB convencional `(0.5, 0.5, 1, 1)`. No material FOX examinado, os componentes usados eram **X no alfa e Y no verde**. Com alfa 1, o mapa anterior reconstruía aproximadamente `(1, 0.01587, 0)`: uma normal deitada, sem alinhamento com a superfície.

A versão corrigida utilizou RGBA aproximadamente `(1, 32/63, 0, 128/255)`. Após compressão e releitura do FTEX, a normal reconstruída ficou próxima de `(0.00392, 0.01587, 0.999866)`, com alinhamento superior a 0,999. O teste reproduziu o erro anterior e aprovou a correção compilada.

Estes canais pertencem ao material e ao recurso examinados. Confirmar o esquema do shader e a codificação efetiva antes de aplicar a outros materiais; não converter todos os mapas cegamente.

### Faces com orientação invertida

Foram corrigidas **87.034 faces exportadas**, rastreadas por posições e normais aos degraus, espelhos e superfícies selecionadas. O conjunto de referência tinha 87.006 triângulos, com 28 correspondências coincidentes adicionais na exportação. Foram ajustados normais e sentido dos índices, mantendo posições, UVs, tangentes e metadados de materiais nessa etapa.

Somente `center1` mudou entre as doze partes FMDL; as outras onze ficaram idênticas. Os dados de iluminação, atmosfera, gramado, torcida e preview foram preservados. O nome `fox3ddf_blin` foi encontrado nas tabelas nativas: não foi trocado por uma hipótese de shader inválido.

A aprovação no jogo refere-se ao resultado conjunto do mapa corrigido e das faces corrigidas. Não houve teste A/B em partida isolando a contribuição visual de cada alteração. Não é justificativa para aumentar potência, mudar exposição universalmente ou importar iluminação de um estádio diferente. O caso anterior da Arena Gramadense, com camada Jbpes, continua sendo outro diagnóstico, não uma solução universal.

## 3. Acabamento e circulação por setor: v39

As muretas e os corredores devem terminar no vão entre arquibancadas e reiniciar no setor seguinte. A proteção lateral precisa conter os degraus: dentes ou espelhos passando para fora da mureta deixam o encontro sem acabamento.

Na v39, as bordas foram recuadas para o lado protegido das **doze muretas laterais**, em seis separações de setores. O teste encontrou 1.142 posições únicas, equivalentes a **2.840 cópias de vértices** nos modelos exportados, que formavam as sobras na v38. Depois da alteração, nenhuma dessas posições excedentes permaneceu. Os vãos, corredores e muros de proteção foram mantidos.

As muretas inferiores e laterais receberam pintura branca envelhecida em **42 objetos**. A textura usa uma célula exclusiva do atlas, ampliado para 2048 × 2048, com pequenas falhas de tinta, variação e sujeira na base. A célula exclusiva evita recolorir outras superfícies que compartilham o atlas. A orientação e o mapa de normais aprovados na v38 foram preservados.

Foram salvas separadamente uma fonte editável com objetos identificados e uma cópia preparada para o jogo. A fonte editável também recebeu as correções de orientação das superfícies para reduzir regressões em futuras preparações. O preflight da cópia não apontou problemas. Os doze FMDL foram relidos no pacote final e comparados por hash; onze partes permaneceram idênticas à v38. A integração conferiu 83 arquivos e substituiu somente o pacote principal e a textura de cores, com backup.

A prévia do Blender foi inspecionada. **O acabamento v39 ainda não foi aprovado em partida.**

## 4. Protocolo reutilizável

1. Confirmar instalação, ID, pasta e versão efetivamente carregados pelo jogo.
2. Preservar a base aprovada e registrar hashes e backups.
3. Rastrear recursos fora de MAIN: common/ad, EXTRA, variantes, LODs e referências FOX.
4. Distinguir visual autorizado, dependência técnica comprovada, herança excedente e conteúdo desconhecido.
5. Para escuridão, comparar a camada efetiva de luz e addons da referência funcional; quando o padrão apontar para superfícies, examinar também canais de normais, orientação e materiais.
6. Alterar somente o escopo comprovado, protegendo LEDs, iluminação aprovada e setores vizinhos.
7. Reproduzir o erro anterior em teste automático e conferir o candidato compilado, não apenas a cena fonte.
8. Reabrir o jogo/Sider e observar as variantes relevantes. Aprovação de uma versão não homologa a seguinte nem outro estádio.

## Evidências locais e limites de publicação

Relatórios preservados no projeto: `teste_normal_packed.json`, `teste_normais_superficies.json`, `testes_no_intruder.json`, `teste_hook_exportador.json`, `testes_BannerServer_Lua.json`, `auditoria_delta_v38.json`, `aprovacao_usuario_v38.json`, `regressao_acabamento_v39.json`, `auditoria_pacote_v39.json`, `integracao_v39.json` e `kirov85_integracao.json`.

Este relato resume essas evidências; os nomes acima não são links para arquivos necessariamente presentes neste repositório. Não foram publicados pacotes proprietários, atlas extraídos, modelos completos, fotos pessoais, logs brutos ou caminhos privados. Os testes descritos foram executados no projeto local; não são testes recém-executados contra o código de cada repositório que recebe este documento.