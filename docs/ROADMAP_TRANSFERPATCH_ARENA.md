# Roadmap e retrospectiva da Transferpatch Arena

**Encerramento do projeto: 5 de novembro de 2026.** A Transferpatch Arena foi construída em Blender e integrada ao SP Football Life 2026 com o Stadium Server do Sider, sobre a base compatível `st052`. Este registro reúne as etapas, as decisões que mudaram o rumo do projeto e o que vale reaproveitar em outros estádios.

## Etapas da construção

| Etapa | Trabalho e resultado |
| --- | --- |
| V01 · Conceito | Definição da arena, da identidade azul-marinho, vermelha e branca e das vistas de interior e exterior. |
| V02 · Entorno | Desenvolvimento da implantação urbana e das vistas aéreas. O entorno recebeu uma revisão posterior de inspiração carioca. |
| V03 · Acessos | Desenvolvimento e verificação em Blender das entradas, rampas, circulação e estacionamento. |
| V1–V4 · Integração e identidade | Preparação e diagnóstico no fluxo PES 2021/Football Life, revisão de texturas e murais e preservação da base do estádio. A V4 teve teste em jogo confirmado pelo responsável pelo projeto. |
| V5 · Interior | Revisão de cadeiras e mosaico, coroa, placares, placas fixas e bordas. As faixas largas receberam repetições ajustadas ao comprimento dos trechos para evitar letras esticadas. |
| V5 · Ajustes finais | O friso superior recebeu faixas contínuas azul-marinho e vermelha, separadas por um detalhe claro. A borda abaixo, escolhida inicialmente para LED, terminou branca. As quatro placas fixas da Transferpatch e do Canal do Prata receberam arte nas duas faces. A miniatura de Amistosos foi atualizada com o DDS fornecido para a arena. |

## Dificuldades e descobertas

- **Estabilidade e integração.** Houve uma etapa de diagnóstico de travamentos durante a preparação para o jogo. A base V4 com teste confirmado serviu de referência para mudanças isoladas. A lição é registrar a última versão que abriu em partida e comparar alterações de geometria, texturas e carregamento separadamente.
- **O mesmo atlas pode servir a mais de uma borda.** O nome do arquivo de textura não bastava para localizar a faixa indicada. A inspeção de materiais, UVs e alturas revelou quatro variantes de publicidade. Em duas, a borda alvo já tinha textura própria. Nas outras duas, ela dividia a malha com um friso vizinho. Separar apenas as faces da faixa alvo permitiu aplicar o branco e preservar o desenho adjacente.
- **Fita de LED superior.** A malha experimental tinha 52 painéis acompanhando a curva, com afastamento de 4 mm. Ela foi incluída em 82 pacotes compatíveis de publicidade. Depois que a fita não apareceu em partida, testamos uma rota específica no Sider. A conferência estática apontava para os arquivos esperados, mas o segundo teste do usuário também não mostrou a fita. A causa em execução não foi isolada. A pedido do usuário, encerramos a tentativa, restauramos os pacotes e a rota anteriores e adotamos a borda branca.
- **Publicidade fixa tem frente e verso próprios.** As novas placas do Canal do Prata ficaram ao lado das placas Transferpatch, com a mesma altura. Para exibir a arte também do gramado, foi preciso orientar as coordenadas de textura do verso para leitura do lado oposto.
- **A prévia depende do carregador ativo.** Nesta instalação, o módulo do Stadium Server usa o DDS `st052.dds` para a arena. O arquivo recebido, de 1364 × 768 em BC1/DXT1, foi decodificado, instalado e comparado byte a byte com o original recebido. A confirmação visual no menu de Amistosos ficou sem registro.

## Fluxo reutilizável

1. Defina a referência visual, o estádio base e uma versão conhecida que entra em partida.
2. Mapeie cada alteração ao modelo, ao material, à textura e ao caminho efetivamente solicitado pelo jogo. Confira variantes e prioridades do Sider.
3. Faça a menor alteração possível, reabra o pacote final e confira os recursos vizinhos. Em superfícies curvas, use a geometria existente como referência para o encaixe e ajuste a repetição da arte ao comprimento real.
4. Instale com backup e compare os arquivos instalados com os arquivos preparados. Separe no registro a prévia do Blender, a verificação dos arquivos e o teste no jogo.
5. Para recursos dinâmicos, comece com um painel e um perfil. Confirme no log o pacote carregado e em partida a reprodução de um ciclo antes de expandir. Se o efeito não aparecer e a causa não puder ser confirmada, preserve uma alternativa estática e reversível.

A fase de construção está encerrada. A verificação dos arquivos finais foi concluída. A confirmação visual em partida da borda branca e das duas faces das placas, assim como a exibição da miniatura no menu, não ficou registrada. A fita de LED superior foi retirada do escopo final.
