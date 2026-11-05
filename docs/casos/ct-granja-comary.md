# CT Granja Comary

Estudo de caso de criação no Blender e integração no Football Life 26. Fechamento editorial em 05/11/2026 sobre a R16, slot 019.

- O entorno foi reconstruído com referências fotográficas e de satélite. Campos, tribuna e heliponto foram preservados como referências durante a revisão dos arredores. A rua atrás da tribuna orientou o eixo do escudo, jardim e caminhos na encosta.
- Para loading infinito, comparar a cadeia efetiva do Sider. Neste caso, retirar a configuração vazia das placas permitiu voltar ao jogo. O roteamento das duas placas LED ficou limitado aos arquivos de geometria do estádio, preservando vídeo e configuração da competição.
- Para linhas flutuantes, exportar a geometria real com identificação de arquivo, malha e faces. A remoção manual identificou 44 de 60 faces em front1.fmdl, malha 21. A remoção pontual foi confirmada em partida.
- O escudo estava acima do terreno na auditoria geométrica. A versão com faces nos dois sentidos foi aprovada em jogo. A causa exata do desaparecimento anterior permanece indeterminada.
- Perfil de transmissão final com altura 0,40 e zoom 14,77 em unidades do controle. Registrar a configuração por estádio.
- A mancha luminosa motivou um mapa especular preto exclusivo para o gramado, mantendo cor e normal. A instalação foi verificada e a eliminação da mancha em jogo permanece sem confirmação específica.
- Manter separados os estados de exportação, integração, teste em partida e aprovação visual. Verificar as referências internas por releitura e preservar uma base com hashes antes de isolar cada defeito.
